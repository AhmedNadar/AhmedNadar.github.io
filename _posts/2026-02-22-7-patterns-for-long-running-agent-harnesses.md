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

Anthropic published a guide called ["Effective Harnesses for Long-Running Agents"](https://docs.anthropic.com/en/docs/build-with-claude/agentic) that distills this into 7 key patterns. I applied them to my harness and found that the first four are easy — most teams nail them naturally. The last three are where agents quietly fail. Here's what each pattern looks like in practice, and what goes wrong when you skip them.

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

The harness maintains a markdown file with checkboxes. Each iteration, the agent finds the first unchecked task and works on it:

```markdown
- [x] FEAT-001: User authentication
- [x] FEAT-002: Landing page with RUI components
- [ ] FEAT-003: Dashboard with stats cards    <-- next task
- [ ] FEAT-004: CRUD for deals
```

Every iteration starts with a fresh context window. The agent doesn't remember what it did last time. Without a file on disk, it has no idea what's been built and what hasn't. It will re-implement features that already exist, or skip features it thinks it already did.

The progress file is the agent's memory between sessions. Cheap to implement, critical to have.

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

After each invocation, the harness checks `git rev-parse HEAD` before and after each invocation. If HEAD changed, a commit happened, which means progress. The commit messages follow a convention:

```
[FEAT-006] Dashboard CRM stats, seed data, 7 system tests
[FEAT-005] Activity model + controller + views, 21 tests
[FEAT-004] Deal model + controller + views, pipeline stages, 20 tests
```

Incrementally committing each task to Git isn't just a log, it's a safety net. If the agent breaks something on iteration 8, you can `git diff HEAD~1` to see exactly what changed. You can revert one commit without losing everything. Without per-task commits, you get one giant diff at the end and no way to isolate what broke what.

---

Those first four patterns are the easy ones. Most of us get them right because they're just good engineering habits applied to automation. The next three are where it gets interesting.

---

## Pattern 5: E2E testing

**The pattern:** Not just unit tests, does the app actually work when you open it in a browser.

A script that discovers every GET route in the app, starts a real Rails server, curls each route, and checks the responses:

```bash
# Discover routes programmatically
ROUTES=$(bin/rails runner "
  Rails.application.routes.routes.select { |r|
    r.verb.to_s.include?('GET')
  }.map { |r|
    r.path.spec.to_s.gsub('(.:format)', '')
  }.reject { |p|
    p.include?(':') || p.start_with?('/rails/')
  }.uniq.sort.each { |p| puts p }
" 2>/dev/null)

# Start a real server on a random port
PORT=$((RANDOM % 1000 + 9000))
bin/rails server -p "$PORT" > /dev/null 2>&1 &

# Check each route
for route in $ROUTES; do
    STATUS=$(curl -s -o /tmp/body -w '%{http_code}' \
             "http://localhost:$PORT$route")
    BODY_SIZE=$(wc -c < /tmp/body | tr -d ' ')

    if [ "$STATUS" = "200" ]; then
        # A 200 with error page content is a lie
        if grep -qiE "(Internal Server Error|something went wrong)" \
           /tmp/body; then
            echo "FAIL $route -- 200 but error page content"
        elif [ "$BODY_SIZE" -lt 100 ]; then
            echo "FAIL $route -- 200 but body too small (${BODY_SIZE}B)"
        fi
    fi
done
```

The failure that made me build this was a route returning HTTP 200 with a Rails error page in the body. The text said "We're sorry, but something went wrong." All unit tests passed. The agent marked the feature as done. When I opened the browser, the page was obviously broken. But the agent never opened a browser — it just saw "200 OK" and moved on.

The key insight: HTTP 200 does not mean "correct." A route can return 200 with a blank page, an error message, or a missing template. Unit tests mock the request cycle — they don't start a real server. The gap between "tests pass" and "app works" is where agents silently fail.

Why not MCP Puppeteer instead? In the Anthropic article, they recommend giving agents browser access via MCP for visual verification. I tried it and found that every MCP server you load costs context before you even call a tool, tool schemas, protocol overhead, capability definitions. These costs are always there, every iteration, whether you use the tools or not. Then Puppeteer adds screenshots on top. Each screenshot costs roughly 3,000-6,000 tokens depending on page size. Check five routes per iteration across ten iterations and you've spent 150,000-300,000 tokens on pictures. Meanwhile, a 10-line text summary of route health gives you the same signal for about 200 tokens. That's a 15-30x difference per check. If you care about your agent's context window, and you should, most MCP servers are not worth loading. Every tool definition you add is a tool the agent has room to think about but less room to think *with*.

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

**The failure that made me build this:** Iteration 4 introduced a subtle bug in a model validation. The tests for that specific model still passed, but it broke a different controller that depended on the validation behavior. Iterations 5, 6, and 7 kept building on top of the broken foundation. Each one added code that *assumed* the validation worked correctly. By the time I noticed — three iterations later — I had to revert three commits and redo the work. Three iterations of wasted LLM calls, wasted tokens, wasted time.

A single `bin/rails test` before each iteration would have caught it immediately. The cost: 15 seconds of test runtime. The savings: three wasted iterations.

## Pattern 7: Prompt efficiency

**The pattern:** Minimize token overhead so the agent has more context for actual reasoning.

**What I do:** The harness pre-computes test results and route health *outside* the LLM call, then injects a compact summary into the prompt:

```bash
build_prompt() {
    local prompt
    prompt=$(cat "$PROMPT_FILE")

    # Run tests outside, inject summary as text
    ./test-summary.sh > /dev/null 2>&1 || true
    if [ -f ".test-summary" ]; then
        prompt="${prompt}

## Current Test State (pre-computed, do not re-run unless you change code)
$(cat ".test-summary")"
    fi

    echo "$prompt"
}
```

The injected summary looks like this:

```
## Test Summary (14:23:07)
34 runs, 84 assertions, 0 failures, 0 errors, 0 skips

## Route Verification (14:23:13)
10 passed, 0 failed (of 10 routes)

OK   / -- 200 (49750B)
OK   /apps -- 200 (12637B)
OK   /users/sign_in -- 200 (14404B)
```

That's about 200 tokens. The same information from the agent running tests itself would be 50-100 lines of raw test output — easily 2,000+ tokens of noise that pushes useful context out.

**The key instruction:** Notice the line "pre-computed, do not re-run unless you change code." Without this, the agent will run the test suite itself anyway — burning tokens on raw output that the harness already summarized. One line of instruction saves thousands of tokens per iteration. Multiply that by 15 iterations and you've saved a significant chunk of your context budget.

More on context window economics in [Part 3](/2026/02/24/context-window-economics-for-ai-agents).

## What I learned

The first four patterns (checklist, progress file, one-feature-per-session, git as memory) are table stakes. You need them, but they're not what makes or breaks your agent. They're just good engineering habits.

The last three are where the real value is:

**Startup health check** catches regressions before they compound. Without it, one broken iteration becomes three broken iterations before anyone notices. The fix is five lines of bash.

**E2E verification** catches the failures that unit tests miss. The gap between "tests pass" and "app works" is real, and agents live in that gap. A script that curls your routes and checks for error pages costs an afternoon to build and catches bugs that would otherwise ship.

**Prompt efficiency** is the least obvious but most impactful. Every token you spend on overhead is a token the agent can't use for thinking. Pre-compute what you can. Inject summaries, not raw output. Tell the agent to trust the summary. The agent that ships the best code isn't the one with the most tools — it's the one with the most tokens left for reasoning.

The easy patterns give you a working agent. The hard patterns give you a reliable one. The difference shows up when the agent has been running for 12 iterations and you check what it shipped.

*Next in the series: [Part 2 — Your agent shouldn't be its own QA: Where verification actually belongs](/2026/02/23/where-verification-belongs-in-agent-harnesses)*
