---
layout: post
title: "Where verification actually belongs in agent harnesses"
date: 2026-02-23
description: "Anthropic says use MCP Puppeteer for screenshots. Geoffrey Huntley says keep verification outside the loop. Who's right? Both, but for different reasons."
tags: [ai, agents, verification, harness]
series: "Building agent harnesses that actually work"
series_part: 2
---

Here's a failure mode that took me too long to catch: the agent marks a feature as "done" because the test passes and the route returns HTTP 200. But the page is blank. Or it shows "We're sorry, but something went wrong" inside a 200 response. Or the layout is completely broken but the controller test doesn't care because it never rendered the actual page.

The agent doesn't know. It can't see the page. It sees a status code and moves on.

This is the E2E verification gap, and there are two competing philosophies on how to close it.

## The problem: HTTP 200 does not mean "correct"

A Rails controller test can pass while the actual rendered page is broken. Here's why:

- Controller tests mock the request cycle. They don't start a real server
- A route can return 200 with a Rails exception page in the body
- A missing partial renders an empty page but no 500 error
- A bad database query returns the layout with no content

I had exactly this situation. A route was returning HTTP 200, all tests passed, and the agent committed the feature as complete. When I opened the browser, the page said "We're sorry, but something went wrong" inside a perfectly valid 200 response. Rails wraps some errors in a 200 because the error page itself renders successfully.

Unit tests tell you the code works. They don't tell you the *app* works.

## Anthropic's recommendation: MCP Puppeteer

Anthropic's guide recommends giving agents browser access via [MCP](https://modelcontextprotocol.io/) (Model Context Protocol). Specifically, they recommend the Puppeteer MCP server, which lets the agent:

1. Navigate to a URL
2. Take a screenshot
3. Analyze the screenshot visually
4. Decide if the page looks correct

The appeal is clear, the agent can *see* what it built. Broken page? The screenshot shows it. Any missing content, it's visible in the image.

I wired this into my harness as an opt-in flag:

```bash
CLAUDE_MCP_FLAGS=""
if [[ "${MCP_PUPPETEER:-}" == "true" ]]; then
    CLAUDE_MCP_FLAGS="--mcp-server puppeteer"
fi
```

I keep it **off by default**. Here's why.

## Geoffrey Huntley's counterpoint: verification outside the loop

Geoffrey Huntley, creator of the [Ralph loop](https://ghuntley.com/ralph), takes a different position. When asked why he doesn't use MCP Puppeteer for visual verification, his answer was one word: [Bombadil](https://github.com/antithesishq/bombadil).

His argument has three parts.

**1. Context window cost.** MCP servers consume tokens just by being loaded, tool definitions, schemas, protocol overhead. Puppeteer adds base64 screenshot data on top of that. Huntley found that MCP servers can drop usable context from ~176k to ~120k tokens. That's 30% of the agent's reasoning capacity gone before it writes a line of code. (I break down the full token budget in [Part 3](/2026/02/24/context-window-economics-for-ai-agents).)

**2. Role confusion.** An agent that codes *and* visually inspects is doing two jobs in one context window. It's like having a developer also be the QA tester. Same screen, same time, half their desk taken up by QA tools.

**3. Separation of concerns.** Verification should be a gate *after* the agent runs, not a tool *inside* the agent's loop. The agent codes. Something else verifies.

## My experience with MCP Puppeteer

I tried it. Here's what happened.

Each screenshot costs roughly 3,000 to 6,000 tokens depending on page size. Over multiple iterations of verifying key routes, this adds up fast, especially before you count the MCP overhead itself, tool schemas, protocol definitions, capability negotiation. Every MCP server you load eats context just by existing, whether you call its tools or not. ([Part 3](/2026/02/24/context-window-economics-for-ai-agents) has the full token math.)

By iteration 5, the agent started forgetting parts of its instructions. By iteration 8, it was re-implementing features it had already built. The context window was so full of screenshot data that the actual task instructions were getting pushed out.

I turned it off and the agent immediately got better. Not because the model improved. Because it had room to think again. Dont use MCP, it eats your context window.

## The fundamental problem

Asking the agent to verify its own work is like asking an author to proofread their own book. They see what they *intended* to write, not what's actually on the page.

| | MCP Puppeteer (inside the loop) | External verification (outside the loop)  |
|---|:---------|:---------|
| **Approach** | "Screenshot /apps, check layout" | "Explore the app, check invariants" |
| **Context cost** | 3-6k tokens per screenshot + MCP overhead | Zero, standalone process |
| **Coverage** | Tests exactly what you script | Can discover paths you didn't think of |
| **Architecture** | Agent = coder + visual QA | Agent = coder. Verifier = QA |
| **Failure mode** | Agent decides its own work looks fine | Independent judgment |

## What I actually built

Bombadil (the tool Huntley points to) is the right direction but not ready for me yet. It's v0.2.1, Linux-only binaries, pre-1.0 API. So I built a pragmatic middle ground, a route verification script that follows Huntley's philosophy, external, zero context cost.

**Step 1: Discover routes programmatically.**

Not by parsing `rake routes` text output, which breaks when column alignment changes between Rails versions, but by querying the Rails router directly:

```bash
ROUTES=$(bin/rails runner "
  Rails.application.routes.routes.select { |r|
    r.verb.to_s.include?('GET')
  }.map { |r|
    r.path.spec.to_s.gsub('(.:format)', '')
  }.reject { |p|
    p.include?(':') || p == '/up' || p.start_with?('/rails/')
  }.uniq.sort.each { |p| puts p }
" 2>/dev/null)
```

**Step 2: Start a real server, hit every route.**

```bash
PORT=$((RANDOM % 1000 + 9000))
bin/rails server -p "$PORT" > /dev/null 2>&1 &
SERVER_PID=$!

# Wait for boot
for _ in $(seq 1 30); do
    curl -sf "http://localhost:$PORT/up" >/dev/null 2>&1 && break
    sleep 0.5
done
```

**Step 3: Check invariants, not screenshots.**

This is the key difference from MCP Puppeteer. Instead of asking "does this page look right?", the script checks a few rules that all working pages must satisfy:

```bash
for route in $ROUTES; do
    STATUS=$(curl -s -o /tmp/body -w '%{http_code}' \
             --max-time 5 "http://localhost:$PORT$route")
    BODY_SIZE=$(wc -c < /tmp/body | tr -d ' ')

    if [ "$STATUS" = "200" ]; then
        # Property: 200 responses must not contain error page content
        if grep -qiE "(Internal Server Error|something went wrong)" /tmp/body; then
            echo "FAIL $route -- 200 but error page content"
        # Property: 200 responses must have substantial content
        elif [ "$BODY_SIZE" -lt 100 ]; then
            echo "FAIL $route -- 200 but body too small (${BODY_SIZE}B)"
        else
            echo "OK   $route -- 200 (${BODY_SIZE}B)"
        fi
    elif [[ "$STATUS" =~ ^(301|302)$ ]]; then
        # Redirects are expected for auth-gated routes
        echo "OK   $route -- $STATUS (redirect)"
    else
        echo "FAIL $route -- HTTP $STATUS"
    fi
done
```

The invariants are simple:
- HTTP status must be 200, 301, or 302
- 200 responses must not contain error page strings
- 200 responses must have a body larger than 100 bytes
- Anything else is a failure

These catch the catastrophic failures, the ones where the agent thinks everything is fine but the app is broken.

## How it integrates into the harness

The verification runs **after** the agent commits, not during the agent's work. The agent never sees the verification tool. It sees the *results*, pre-computed and injected as text on the next iteration:

```bash
# Post-commit route verification (non-blocking warning)
if [ -x "./verify-routes.sh" ]; then
    ./verify-routes.sh 2>/dev/null || \
        echo "WARNING: route verification failed"
fi
```

The results get injected into the next iteration's prompt:

```
## Route Verification (02:08:13)
10 passed, 0 failed (of 10 routes)

OK   / -- 200 (49750B)
OK   /apps -- 200 (12637B)
OK   /users/cancel -- 302 (redirect)
OK   /users/sign_in -- 200 (14404B)
OK   /users/sign_up -- 200 (14980B)
```

Ten lines. About 200 tokens. The agent sees route health the same way it sees test results, pre-computed facts, not a tool to invoke. Compare that to thousands of tokens for a single MCP screenshot.

## The gotchas I hit building this

**Puma 7 removed the `-d` (daemonize) flag.** I had to background the process with `&` and manage the PID manually.

**`bin/rails server` refuses to start if `tmp/pids/server.pid` exists.** If a previous run crashed without cleanup, the next run fails silently. I use a separate PID file path: `-P tmp/pids/verify.pid`.

**Turbo's `*_historical_location` routes return 13-14 byte bodies.** These are redirect stubs by design, not errors. I filter them out to avoid false positives.

**Auth-gated routes return 302, not 401.** Devise redirects to sign-in instead of returning unauthorized. Expected behavior, not failures. The script counts 302 as a pass.

## The principle

Separation of concerns applies to agents too:

- The coding agent codes.
- A separate tool verifies.
- The harness orchestrates both.

Putting coding and verification in one context window is like asking a developer to write code and manually test it on a split screen. Half their monitor taken up by browser devtools. They *can* do it. But they'll do both jobs worse than if they focused on one.

***Next**: [Part 3 — Context window economics for AI agents](/context-window-economics-for-ai-agents)*
