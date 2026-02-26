---
layout: post
title: "7 Patterns for long-running agent harnesses"
date: 2026-02-22
description: "What I learned applying Anthropic's 7 harness patterns to my agent build system. The easy ones don't matter much — the hard ones are what separate shipping from spinning."
tags: [ai, agents, rapidfy, harness]
series: "Building agent harnesses that actually work"
series_part: 1
---

Most teams building with AI agents focus on the agent — the model, the prompt, the system instructions. But the thing that determines whether your agent *actually ships working code* isn't the agent itself. It's the **harness**, aka: "the loop around it".

This harness decides what the agent sees, when it stops, what gets committed, and how failures are caught. It's like the "controller" in a Rails application. Get it wrong and your agent writes code that looks correct but breaks everything because it was built on wrong assumptions.

I build Rapidfy, an agentic system that generates full Rails applications from a spec. The agent (an LLM with tools) writes the code. The harness (a bash loop) orchestrates: prepare the prompt, invoke the agent, check if progress was made, loop or stop.

Anthropic published a guide called ["Effective Harnesses for Long-Running Agents"](https://docs.anthropic.com/en/docs/build-with-claude/agentic) that distills this into 7 key patterns. I applied them to my harness and found that the first four are easy, most developers nail them naturally. The last three are where agents quietly fail. Here's what each pattern looks like in practice, and what goes wrong when you skip them.

## Pattern 1: Feature checklist (JSON)

**The pattern:** Give the agent a machine-readable list of what to build, with pass/fail tracking.

My harness reads a JSON spec file with structured acceptance criteria. Each feature has a test command and a `passes` field:

```json
{
  "features": [
    {
      "id": "FEAT-001",
      "title": "User authentication",
      "criteria": [
        {
          "description": "User can sign up with email/password",
          "test": "bin/rails test test/integration/signup_test.rb",
          "passes": false
        }
      ]
    }
  ]
}
```

The `test` field is what makes this work. Without it, the agent decides *on its own* whether a feature is done, and agents are optimistic. They'll mark a feature complete because the code compiles, not because it actually works. With an explicit test command, the harness can verify: run this command, did it pass? Yes or no.

Without it the agent builds features in whatever order it feels like. It skips hard ones. It marks things "done" based on vibes. You end up with 8 features "completed" and 3 of them broken.

## Pattern 2: Progress file

**The pattern:** Keep a persistent record of what's done on disk, not in conversation history.

The harness maintains a markdown file with checkboxes. In each iteration the agent finds the first unchecked task and works on it and checks it off.

```markdown
- [x] FEAT-001: User authentication
- [x] FEAT-002: Landing page with RUI components
- [ ] FEAT-003: Dashboard with stats cards    <-- next task
- [ ] FEAT-004: CRUD for deals
```

Every iteration starts with a fresh context window. The agent doesn't remember what it did last time. Without a file on disk, it has no idea what's been built and what hasn't. It will re-implement features that already exist, or skip features it thinks it already did. That's why the progress file is the agent's memory between sessions. Cheap to implement, critical to have.

## Pattern 3: One feature per session

**The pattern:** The agent does ONE thing per invocation, then exits.

The harness enforces this with a circuit breaker. After each invocation, it checks if the git HEAD changed (meaning the agent committed something). If three iterations pass with no commit, the harness exits:

```bash
if [[ "$(git rev-parse HEAD)" == "$BEFORE" ]]; then
    NO_CHANGE=$((NO_CHANGE + 1))
    echo "No commit ($NO_CHANGE/3)"
    (( NO_CHANGE >= 3 )) && { echo "Circuit breaker."; exit 1; }
else
    NO_CHANGE=0
fi
```

I learned this one the hard way. Without a circuit breaker, the agent once ran 12 iterations without committing anything. It kept editing the same file over and over, convinced each edit was progress. Each iteration cost tokens and time, producing nothing. Twelve iterations of an LLM spinning on the same problem is expensive and pointless.

The circuit breaker forces the agent to either ship something or stop trying. Three attempts is generous. If it can't make progress in three tries, a human needs to look at it.

## Pattern 4: Git as memory

**The pattern:** Each task = one commit. The git log becomes a readable history of what the agent built.

The harness checks `git rev-parse HEAD` before and after each invocation. If HEAD changed, a commit happened, which means progress. The commit messages follow a convention:

```
[FEAT-006] Dashboard CRM stats, seed data, 7 system tests
[FEAT-005] Activity model + controller + views, 21 tests
[FEAT-004] Deal model + controller + views, pipeline stages, 20 tests
```

Incrementally committing each task to Git isn't just a log, it's a safety net for when the agent breaks something. If the agent breaks something on iteration 8, you can `git diff HEAD~1` to see exactly what changed. You can revert one commit without losing everything. Without per-task commits, you get one giant diff at the end and no way to isolate what broke what.

---

Those first four patterns are the easy ones. Most of us get them right because they're just good engineering habits applied to automation. The next three are where it gets interesting.

---

## Pattern 5: E2E testing

**The pattern:** Not just unit tests, does the app actually work when you open it in a browser.

A route returning HTTP 200 with a Rails error page in the body was a common failure mode for my agent. All unit tests passed. The agent marked the feature as done. But the page said "We're sorry, but something went wrong", wrapped in a valid 200 response that the agent never visually checked.

HTTP 200 does not mean "correct." The gap between "tests pass" and "app works" is where agents silently fail. Closing that gap requires a manual verification step outside the agent's loop that starts a real server, hits every route, and checks the responses for actual content, not just status codes.

I built a route verification script that does exactly this, and tried Anthropic's MCP Puppeteer approach as an alternative. The tradeoffs between the two are more interesting than the script itself. [Full implementation and comparison in Part 2](/where-verification-actually-belongs-in-agent-harnesses).

## Pattern 6: Startup health check

**The pattern:** Before starting new work, verify nothing is already broken.

Before each iteration, I run the full test suite, test it visually, and check the route health. I do manual verification because I want to catch failures that unit tests miss. And if any test fails, I stop immediately:

```bash
if [[ "$MODE" == "build" ]]; then
    HEALTH=$(bin/rails test 2>&1) || true
    if echo "$HEALTH" | grep -qE '[1-9][0-9]* (failures|errors)'; then
        echo "Health check failed. Fix failures before continuing."
        exit 1
    fi
    echo "Health check passed."
fi
```

Note the regex: `[1-9][0-9]* (failures|errors)`. It matches "1 failures", "3 errors", "12 failures" — but *not* "0 failures". That distinction matters. `grep "failures"` would match the success line too.

While building this harness, I hit a wall where iteration 4 introduced a subtle bug in a model validation. The tests for that specific model still passed, but it broke a different controller that depended on the validation behavior. Iterations 5, 6, and 7 kept building on top of the broken foundation. Each one added code that *assumed* the validation worked correctly. By the time I noticed — three iterations later — I had to revert three commits and redo the work. Three iterations of wasted LLM calls, wasted tokens, wasted time. This was a failure mode that could have been caught much sooner if I had run the full test suite before each iteration.

A single `bin/rails test` before each iteration would have caught it immediately. The cost: 15 seconds of test runtime. The savings: three wasted iterations.

## Pattern 7: Prompt efficiency

**The pattern:** Minimize token overhead so the agent has more context for actual reasoning.

When building this harness, I learned that pre-computing what you can outside the LLM call, then injecting compact summaries into the prompt is the most impactful pattern. Test results, route health, linting output — anything that can be summarized in a few lines of text shouldn't be produced by the agent itself. Raw test output is 50-100 lines. A summary is 3-15 lines. That difference compounds across every iteration.

This turned out to be the least obvious pattern but the most impactful. [Full implementation and token math in Part 3](/context-window-economics-for-ai-agents).

## What I learned

The first four patterns (checklist, progress file, one-feature-per-session, git as memory) are table stakes. You need them, but they're not what makes or breaks your agent. They're just good habits.

The last three are where the real value is. **Startup health check** catches regressions before they compound — five lines of bash that save entire iterations of wasted work. **E2E verification** catches the failures that unit tests miss. **Prompt efficiency** determines how long your agent stays useful before it starts forgetting its instructions.

The easy patterns give you a working agent. The hard patterns give you a reliable one. Parts 2 and 3 dig into the two hardest: verification and context economics.

***Next**: [Part 2 — Where verification actually belongs in agent harnesses](/where-verification-actually-belongs-in-agent-harnesses)*
