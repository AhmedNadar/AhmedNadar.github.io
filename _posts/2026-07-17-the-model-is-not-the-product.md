---
layout: post
title: "The Model Is Not the Product. The Harness Is."
date: 2026-07-17
description: "An n8n version bump stopped enterprise workflows dead this year, and the model was never the thing that broke. What separates agents that ship from agents that demo is the deterministic infrastructure wrapped around them."
tags: [AI, agents, harness, engineering, production]
---

Teams that upgraded n8n from v2.4.7 to v2.6.3 this year watched their production workflows stop. The Vector Store Question Answer Tool started emitting tool schemas with a type of `None` instead of `object`, and both OpenAI and Anthropic rejected the calls outright. OpenAI returned `schema must be a JSON Schema of 'type: "object"'`. Anthropic returned `input_schema.type: Field required`. The fix was rolling back a version across infrastructure already serving real customers.

Nothing was wrong with the models. The models never saw the request. A tool definition drifted out of shape during a routine version bump, and everything downstream of it stopped being an AI system and started being an outage.

That is the shape of almost every agent failure I have watched, and it is why I think the industry is looking at the wrong layer. The frontier in 2026 is not prompt design, not fine-tuning, not retrieval tuning. It is [harness engineering](/7-patterns-for-long-running-agent-harnesses/): the tool orchestration, verification loops, context management, guardrails, and observability that make an agent survivable under production load. Teams shipping agents at scale have converged on the same formula, that an agent is a model plus a harness. Strip the harness and what remains is a stochastic actor with no executive layer, no error recovery, and no way to tell a good decision from a confidently stated wrong one.

The n8n case was the merciful version, because it broke loudly. The expensive failures do not throw. A tool call returns malformed JSON, and the model, rather than raising an exception, improvises around the broken response and carries on with corrupted context. No error. No log line. The monitoring dashboard stays green while users get wrong answers, and the degradation compounds across sessions until a human reads an output and notices something is off.

Multi-agent architectures give you a bigger version of the same problem. One agent hands incomplete context to the next. The receiving agent produces something confident and wrong. The pipeline continues, the next agent builds on that wrong foundation, and by the time the error surfaces it has been amplified by every step in between. Reconstructing what happened means tracing the full decision path across every model call, every tool invocation, every retrieval. Without that trace, debugging is archaeology.

Anthropic published a three-agent harness in April that takes this seriously. One agent plans, turning a prompt into a specification. One generates, building in chunks. One evaluates, testing the running product with tools like Playwright and filing precise bug reports. The reasoning behind the split is the part worth stealing: a model grading its own output skews positive, and as a context window fills, coherence degrades and the model starts wrapping up work early because it senses it is running out of room. A separate skeptical evaluator gives the generator something to push against, and the agents coordinate through contracts that define what done means before any code gets written.

<!-- IMAGE: diagram of a three-stage harness, plan then generate then evaluate, with the verification gate between steps. Add to /assets/images/posts/ and reference as {{ site.url }}/assets/images/posts/the-model-is-not-the-product.png -->

I built [Rapidfy](/agentic-rapid-rails-app-in-second/) around the same instinct, the one I argued for in [where verification belongs](/where-verification-actually-belongs-in-agent-harnesses/). The harness checks the agent's plan before execution, not after. Between every significant step it asks three questions: does this proposed action violate a known constraint, does the last step's output match the expected schema, is the agent still inside [the context budget](/context-window-economics-for-ai-agents/) the task was given. If any check fails, the loop stops and flags it rather than letting the agent proceed with corrupted context and a confident tone.

That costs ten to fifteen seconds per significant step, and I took the trade on purpose. When an agent spends forty minutes down a wrong path because nobody checked its plan at step three, the ten-second check at step three was cheap. The failure I was designing against was never the spectacular crash. It was the plausible, confident, wrong answer at the end of a long run, the one that reads as correct until somebody who knows the domain notices it took a wrong turn fifteen steps back.

The observability gap cuts the same way. Non-deterministic systems rarely crash. They degrade. They loop. They pick the wrong tool on the fourth call instead of the first. They make decisions on incomplete context, and that wrongness shows up as slightly worse output, slightly higher latency, slightly odd cost. Each signal on its own reads as noise. Only the trace shows the pattern. The teams winning with agents in production instrument every step, not to find bugs in advance, but to reconstruct what happened when something goes wrong, and something always goes wrong.

Gartner expects more than forty percent of agentic AI projects to be canceled by the end of 2027. That number gets quoted as a verdict on AI, and it is the wrong reading. It is a forecast about teams that shipped agents without harnesses and are now paying the compounding cost of that choice. The models are not the problem. The missing engineering discipline is.

None of this work is glamorous. A verification loop does not demo well. Observability infrastructure is not a feature anyone requests at a roadmap meeting. Context budget management has never made a conference keynote. The harness is the invisible layer that makes the visible layer trustworthy, and invisible layers are the first thing cut when a deadline moves.

The model is what gets the demo. The harness is what survives the version bump.
