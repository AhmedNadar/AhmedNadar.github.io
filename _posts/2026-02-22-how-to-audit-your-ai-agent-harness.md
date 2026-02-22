---
layout: post
title: "How to audit your AI agent harness (A Practical Checklist)"
date: 2026-02-22
description: "Step-by-step guide to auditing your AI agent harness against Anthropic's 7 patterns for long-running agents, with a worked example from Rapidfy."
tags: [ai, agents, rapidfy, harness]
series: "Building agent harnesses that actually work"
series_part: 1
---

Most teams building with AI agents focus on the agent, the model, the prompt, and the system instructions. But the thing that determines whether your agent *actually ships working code* isn't the agent itself. It's the **harness**, the loop around it.

The harness decides what the agent sees, when it stops, what gets committed, and how failures are caught. Get it wrong and your agent writes code that looks correct but breaks everything because it was built on wrong assumptions.

Anthropic published a guide called "Effective Harnesses for Long-Running Agents" that distills this into 7 key patterns. We used it to audit the harness we built around LLMs for building Rapidfy applications and found that scoring well on *most* patterns doesn't mean much. It's the 2-3 hard ones that separate harnesses that ship from harnesses that spin.

## Anthropic's 7 Patterns

Here's the checklist, distilled from Anthropic's guide:

| Pattern | What It Means |
|:---------|:---------------|
| Feature checklist (JSON) | Machine-readable list of what to build, with pass/fail tracking |
| Progress file | Persistent record of what's done, survives context resets |
| One-feature-per-session | Agent does ONE thing per invocation, then exits |
| Git as memory | Each task = one commit. Git log = agent memory |
| E2E testing | Not just unit tests — does the app actually work? |
| Startup health check | Before starting new work, verify nothing is broken |
| Prompt efficiency | Minimize token overhead so the agent can think |

## Worked example

Our harness is a bash loop that reads a specification file, invokes an LLM, builds one feature at a time, and checks for a git commit after each invocation. The agent decides what code to write, what tests to run, what to fix. The harness just orchestrates: prepare prompt, invoke, check progress, loop or stop. Let's look at each pattern in detail.

### Pattern 1: feature checklist

The harness uses a `checklist.json` file with structured acceptance criteria and pass/fail tracking:

```json
{
  "features": [
    {
      "id": "feature-id",
      "title": "feature title",
      "criteria": [
        {
          "description": "criterion description",
          "test": "test command to verify the criterion",
          "passes": false
        }
      ]
    }
  ]
}
```

The `passes` field is machine-readable, the harness can programmatically check how many features are done. The `test` field tells the LLM *exactly* how to verify each criterion.

### Pattern 2: progress file

The harness maintains a checklist of tasks. Each iteration, the LLM finds the first `- [ ]` and works on it. This survives context window resets because it's a file on disk, not in the conversation.
After committing, it checks it off:

```markdown
- [x] feature-id
- [ ] feature-id
```


### Pattern 3: one feature per session

The harness enforces this with a circuit breaker. The agent builds one feature per invocation. If it fails to make progress after 3 consecutive iterations, the harness exits.

### Pattern 4: git as memory

Every iteration checks for a new commit. If the HEAD changed, progress was made. The commit message follows a convention (`[feature-id] description`) that makes the git log a readable history of what the LLM built.

### Pattern 5: E2E testing

A unit test can pass while the actual app doesn't work. We built a script that starts a real server, curls every route, and checks that responses are sane — not blank, not error pages hidden behind HTTP 200. Full browser-based E2E via MCP burns too many tokens. A script-based approach outside the agent loop is more practical for long-running agents.

### Pattern 6: startup health check

Simply run the test suite *before* starting work to catch regressions early.

### Pattern 7: prompt efficiency

The harness pre-computes test results outside the LLM call and injects them as a compact text summary.


## The 3 patterns that matter most

**1. startup health check** - run your test suite *before* each agent iteration. If something is broken, stop.

**2. e2e verification** - "Tests pass" is necessary but not sufficient. Start a real server, hit real routes, check real responses.

**3. prompt efficiency** - every token you waste on overhead is a token the agent can't use for reasoning. Pre-compute expensive checks outside the loop, inject results as compact text.


The easy patterns give you a working agent. The hard patterns give you a *reliable* one.

*Next in the series: Part 2 — Your agent shouldn't be its own QA: Where verification actually belongs*