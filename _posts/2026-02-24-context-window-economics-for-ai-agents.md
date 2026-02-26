---
layout: post
title: "Context window economics for AI agents"
date: 2026-02-24
description: "Every tool, MCP server, and progress file you add to an agent costs tokens. Most teams don't measure this. Here's how to think about context allocation."
tags: [ai, agents, context-window, harness]
series: "Building agent harnesses that actually work"
series_part: 3
---

Your agent has a 200k token context window. You might think that means 200k tokens of useful work. It doesn't.

After model overhead, system prompts, tool definitions, autocompact buffer, memory files, SKILL files, and accumulated conversation history, you might have half that for actual reasoning. Add an MCP server or two and you're down to a third.

Many developers building with agents never measure this. They add tools, extend prompts, wire up MCP servers, and wonder why the agent starts forgetting instructions halfway through a task. The answer is always the same, you're taxing your agent's brain and not tracking the bill.

## Where tokens actually go

Here's a rough breakdown from my agent system:

| Category | Approximate cost | Notes |
|:---------|:----------------|:------|
| Model overhead | ~24k tokens | System prompt, safety instructions, built-in tool definitions |
| Your harness prompt | ~3-10k tokens | Instructions, spec file, task list |
| MCP server (each) | ~20-30k tokens | Tool schemas, protocol overhead, response data |
| Conversation history | Grows per turn | Every tool call + response accumulates |
| **Available for reasoning** | **What's left** | This is what determines agent quality |

A 200k context window with two MCP servers and a detailed prompt might leave 100-120k for actual work. That sounds like a lot. Then the agent hits iteration 8 and the conversation history has consumed another 60k.

I measured this by accident. My agent was performing well for 4-5 iterations, then quality would drop sharply. Features that should have taken one iteration started taking three. The agent would forget constraints from its instructions and start writing code that violated my coding standards.

The problem wasn't the model. The problem was that by iteration 6, there wasn't enough room left in the context window for the model to hold the instructions *and* reason about the code *and* remember what it had already done.

## The MCP tax

MCP servers are the biggest hidden cost in agent architectures. Each server adds three layers of overhead:

**Tool definitions.** Every tool the server exposes gets serialized as a JSON schema in the context. A server with 10 tools might add 3,000-5,000 tokens just for the definitions. Before you ever call a tool.

**Protocol overhead.** Handshake, capability negotiation, connection state. Small per-call, but it adds up across dozens of tool invocations per iteration.

**Response data.** This is the killer. Tool outputs go directly into the context window. For most tools, responses are small (a file listing, a search result). For Puppeteer, a single screenshot costs roughly 3,000 to 6,000 tokens, depending on page size.

### What MCP Puppeteer actually costs

I ran this experiment. One verification check, "are the routes healthy?", in two formats.

**MCP Puppeteer (inside the agent loop):**

```
Agent: [tool_call] puppeteer_navigate { url: "http://localhost:3000/" }
Server: { status: "navigated", title: "Rapidfy" }
Agent: [tool_call] puppeteer_screenshot {}
Server: { screenshot: "data:image/png;base64,iVBORw0KGgoAAAA..." }
         ^^^ 3,000-6,000 tokens for ONE screenshot
Agent: "The page looks correct, I can see the header and navigation..."
```

Repeat for 5-10 routes per iteration. Total: **15,000-60,000 tokens per verification pass**, plus the MCP overhead that's always there whether you call a tool or not.

**Text summary (outside the agent loop):**

```
## Route Verification (14:23:13)
10 passed, 0 failed (of 10 routes)
OK   / -- 200 (49750B)
OK   /apps -- 200 (12637B)
...
```

Total: **~200 tokens.** Same signal. The agent knows all routes are healthy. It doesn't need to *see* the pages to know they render.

That's a 75-300x difference for the same information.

## The one-window-one-goal principle

Geoffrey Huntley's [harness philosophy](https://ghuntley.com/ralph) can be summarized in one line:

> One window, one goal. Reset between iterations.

This means:
- Each agent invocation gets a **fresh** context window
- The agent does ONE thing (implement one feature, fix one bug)
- All state lives **on disk** (files, git), not in conversation history
- Tools are minimal: code editing and shell access, nothing more

My harness implements this literally. Each iteration is a fresh invocation:

```bash
OUTPUT=$(build_prompt | claude -p --dangerously-skip-permissions 2>&1 \
         | tee "$LOG" || echo "ERROR")
```

The prompt is piped in. The agent runs. Output is captured. The loop checks for progress and either continues (fresh invocation) or stops. No accumulated conversation history across iterations.

This is why the circuit breaker from [Part 1](/7-patterns-for-long-running-agent-harnesses) matters, each iteration starts clean, so the agent can't spiral. And it's why the progress file matters, since context resets between iterations, the agent needs something on disk to tell it where it left off.

## The test-summary injection pattern

This is the single most impactful optimization in my harness. Instead of the agent running `bin/rails test` itself (which fills the context with 50-100 lines of raw test output), the harness pre-computes the results and injects a compact summary.

```bash
build_prompt() {
    local prompt
    prompt=$(cat "$PROMPT_FILE")

    # Run tests OUTSIDE the agent, inject summary
    ./test-summary.sh > /dev/null 2>&1 || true
    if [ -f ".test-summary" ]; then
        prompt="${prompt}

## Current Test State (pre-computed, do not re-run unless you change code)
$(cat ".test-summary")"
    fi

    echo "$prompt"
}
```

The test-summary script extracts just the stats and failures from raw test output:

```bash
RAW=$(bin/rails test 2>&1) || true

# Extract summary line (e.g. "34 runs, 84 assertions, 0 failures")
STATS=$(echo "$RAW" | grep -E '^\d+ runs,' | tail -1)

# Extract failure lines only (compact)
FAILURES=$(echo "$RAW" | grep -E '(Failure|Error):$' -A 2 | head -30)
```

Full test output: 80 lines. Summary: 3-15 lines. That's an 80-90% token reduction on test state, every single iteration.

**The instruction that saves the most tokens:**

```markdown
## Orientation
- If a "Current Test State" section is appended below, use it to orient —
  don't re-run tests unless you change code
- Focus on failures listed in the summary first
```

Two lines. Without them, the agent re-runs the entire test suite on its own, wasting tokens on raw output that the harness already summarized. With them, the agent trusts the pre-computed results and only runs tests after making changes.

Over 15 iterations, this saves roughly 25,000-40,000 tokens. That's the difference between an agent that finishes its task and one that starts forgetting its instructions on iteration 10.

## What to keep inside vs outside the context

Here's the framework I use:

### Keep inside the context window
- **Task definition:** what to build, acceptance criteria, test commands
- **Relevant code:** files the agent needs to read and modify (on demand, not pre-loaded)
- **Pre-computed state:** test results, route health, as compact text
- **Conventions:** coding standards, commit format, project rules (short list)

### Externalize
- **Raw test output:** run tests outside, inject a summary
- **Route verification:** check outside, inject pass/fail counts
- **Screenshots:** don't. Use text-based checks instead
- **Full file contents:** let the agent read files on demand rather than front-loading everything

The general principle, **if it can be computed outside the context window and summarized in under 20 lines of text, do it outside.**

## Practical strategies

**1. Pre-compute everything you can.** Test results, route checks, linting output. Run it in the harness, summarize it, inject it as text. The agent should consume results, not produce them.

**2. Use compact formats.** `10 passed, 0 failed` beats a full test log. `OK /apps -- 200 (12637B)` beats a screenshot. Design your summaries for scanability.

**3. Reset between iterations.** Don't accumulate conversation history across agent invocations. Each iteration starts with a fresh context window and a current state snapshot from disk.

**4. Measure your overhead.** Count the tokens in your system prompt, tool definitions, and injected context. If overhead exceeds around 40% of your context window, you're leaving performance on the table.

**5. Question every MCP server.** For each MCP server in your agent's config, ask: "Can this be replaced with a shell script that produces a text summary?" If yes, do it. The token savings compound across every iteration.

**6. Tell the agent what not to do.** "Do not re-run tests unless you change code" saves more tokens than any optimization technique. Agents are eager. They'll redo work to be safe. Explicit instructions to trust pre-computed state prevent this.

## The optimization that matters

The best agent optimization isn't faster models or bigger context windows. It's spending fewer tokens on overhead so more tokens go to actual thinking.

Every MCP server you add is a tax on your agent's ability to reason about the problem. Every raw test log you inject is noise that pushes out signal. Every screenshot is thousands of tokens that could have been ten lines of text.

The agent that ships the best code isn't the one with the most tools. It's the one with the most tokens left for reasoning.


*This concludes the series "Building agent harnesses that work." 
Start from [Part 1 — 7 Patterns for long-running agent harnesses](/7-patterns-for-long-running-agent-harnesses) or revisit [Part 2 — Where verification actually belongs in agent harnesses](/where-verification-actually-belongs-in-agent-harnesses).*
