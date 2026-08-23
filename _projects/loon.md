---
layout: project
title: LOON
description: "A voice-driven AI desktop assistant for macOS. Talk to it, and it handles the boring parts of your digital life so you can focus on humans."
link: https://heyloon.com
order: 3
status: "Building toward v1"
---

> **For people who want help, not another app to learn.**
> Hold a key, talk to LOON, and it handles email, calendar, social media, research, and the rest of the daily digital chores. Built for grandparents, teachers, nurses, kids, and anyone tired of clicking through 20 tabs.

<p class="cta-row">
  <a href="https://heyloon.com" class="cta-button">Visit heyloon.com &rarr;</a>
</p>

---

## The job everyone has but nobody loves

Email triage. Calendar juggling. Replying to a LinkedIn DM you have been ignoring for three days. Drafting a polite reply to a parent. Remembering your grandkid's birthday next week.

These are not hard. They are just numerous, repetitive, and they steal hours from the work that actually matters.

The current answer is "use 12 different apps and stay organized." That works for power users. It does not work for grandma.

LOON is the simpler answer. **One voice, one assistant, one privacy promise.**

---

## How it works today

You hold a key. You talk. LOON responds, runs an action, and tells you what it did.

- **Voice in, voice out.** Push to talk via Deepgram. Local TTS for replies. Hands-free.
- **Computer control.** LOON drives Mac apps via Apple's accessibility APIs. Opens Mail, Calendar, Notes, Safari, Slack, etc.
- **Browser automation.** Fills forms, reads pages, posts on Twitter or LinkedIn through your existing logged-in sessions.
- **17 bundled skills.** PDF, Excel, PowerPoint, Word, design, web scraping, Telegram, social posting, deep research, travel planning, more.
- **Your data stays on your Mac.** SQLite under `~/Library/Application Support/LOON/`. No cloud sync, no telemetry, no surveillance.
- **Bring your own LLM key**, OR pay the bundled subscription and never see an API key.

---

## How it works tomorrow (v2: the multi-agent caravan)

LOON v1 is one agent. LOON v2 is many.

I am building **Karwan** (Persian for "caravan"): an orchestrator that delegates work to specialist sub-agents in parallel. Email Agent reads your inbox while Twitter Agent posts your draft while Research Agent compiles your sources, all at once.

Each specialist has:
- Its own SOUL: identity, scope, refusal patterns, guardrails
- Its own browser session (no more "browser is busy" collisions)
- Its own model + max turns + budget
- A Critic Agent that reviews drafts before anything ships

Karwan is the soul. The specialists are the hands. You keep the eyes.

---

## Who it is for

LOON is built for four audiences. Each gets a curated default set of skills and behavior.

- **Teachers.** Lesson plans, parent emails, rubrics, accommodations.
- **Nurses.** SBAR handoffs, medication reference (with disclaimers), shift summaries.
- **Elderly.** Voice-first, large fonts, family birthday reminders, pre-authorized helpers (e.g. "buy a $20 gift for my grandkid Tom for his birthday next week, ship to his address").
- **Kids.** Homework helper that asks Socratic questions, reading aloud, content filter.

And of course **power users** like me, who get all of it plus their own LLM key.

---

## Why I am building this

I have built civic tools (SolveTO), contractor tools (STL Agent), shipping tools (Rapid Rails). The thread connecting them: **software should remove friction, not add it.**

LOON is the same idea, applied to the most overlooked user: the non-technical person trying to keep up with a digital life that was designed for engineers.

My grandmother does not need an AI agent. She needs someone who already knows her son's address, knows her grandkids' birthdays, and can quietly handle the steps between her intent ("send Tom a birthday card") and the outcome (a card showing up at his house). LOON is that someone.

---

## Tech under the hood

- **Swift / SwiftUI** for the macOS app
- **Anthropic Claude** (Sonnet, Opus, Haiku) as the reasoning engine
- **MCP (Model Context Protocol)** for tool extensibility
- **macos-use MCP** for OS control
- **Playwright** for browser automation
- **GRDB / SQLite** for local data
- **Postmark + Stripe** (planned) for license + email
- **Hetzner** for hosting (no Google Cloud, no AWS lock-in)
- **Rails 8** for the heyloon.com website + license backend

Forked from the open-source [Fazm](https://github.com/mediar-ai/fazm) engine by Mediar AI Inc., heavily rebranded and customized for ConnectRoots Inc. Karwan multi-agent orchestration is original work.

---

## Status

**v1 in active development.** The desktop app runs, voice works, skills work, browser automation works. Working through final polish, license backend, website, and Apple Developer Program enrollment for distribution.

**Public launch target: Q3 2026.**

If you want early access, [contact me](/contact/) and tell me which audience you fit (teacher, nurse, elderly family member, dev, etc).

<p class="cta-row">
  <a href="https://heyloon.com" class="cta-button">heyloon.com &rarr;</a>
</p>
