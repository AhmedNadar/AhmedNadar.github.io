---
layout: page
title: About
permalink: /about/
description: "Toronto-based product builder with more than 20 years building software. I ship Rails apps that solve a specific problem for a specific person. Currently building Solve Canada and STL Agent."
---

<div class="about-intro" markdown="0">
  <img class="about-intro-photo" src="{{ '/assets/images/ahmed-nadar.jpg' | relative_url }}" alt="Ahmed Nadar, photographed against the Toronto skyline" width="168" height="168" loading="eager" decoding="async">
  <div class="about-intro-text">
    <p>I'm Ahmed. I build Rails apps in Toronto that solve a specific problem for a specific person. I have spent more than 20 years building software, from interface design through to the systems underneath.</p>

That whole time I have been doing the same thing: figuring out what a real human actually needs and turning it into something that works. The tools changed. The work didn't. What did change is how fast one developer can now ship the thing, and how much that speed reshuffles who gets to build what.
  </div>
</div>

## What I'm building right now

**[Solve Canada](https://solvecanada.ca)** is the reporting layer for any Canadian city. I started it in Toronto in January 2026, after watching a pothole on my own street go unreported for months because saying something took longer than ignoring it. It now runs in seven Canadian cities: Toronto, Mississauga, Milton, Kitchener, Waterloo, Ottawa, and Peterborough. Each city keeps its own front door, and **[SolveTO](https://solveto.ca)** is the Toronto one. It does not stop at 311 either, because the same city owns the parks, the schools, the transit stops, and the water. Point at any of it, take a photo, and AI writes a report the right department can act on, your councillor sees it, it lands on a public map, and neighbours can verify it. **Over three million pieces of public infrastructure across seven cities. Over a thousand reports filed since February. One account that follows you across city lines. Five media interviews in the first week.** Built ~90% with AI-assisted development on Rails 8.

**[STL Agent](/projects/stl-agent/)** — speed-to-lead for GTA contractors. Average GTA contractor takes 42 hours to reply to a quote request. The customer who responds first wins 78% of the time. STL pings the contractor on Telegram in under 60 seconds, AI emails the customer back, and the whole CRM lives inside Telegram so the contractor stays on the roof. Currently booking 5 GTA contractors for white-glove install.

**[RapidRails](https://rapidrails.cc)** — the production-ready UI component library both products are built on. Every component I kept rebuilding across client projects, packaged into a Ruby gem. ViewComponent + Stimulus + Tailwind. No React, no JS build steps.

## How I actually work

It starts with a real conversation with a real human. I asked Toronto residents what was annoying about reporting issues — most of them had never used 311 because the categories don't match how they think. So SolveTO's photo-first flow exists because that conversation happened first, not because it sounded innovative.

I asked plumbers, electricians, and roofers what was killing their conversion rate. The answer was always the same: "I'm on a job, my phone's in the truck, by the time I see the form the customer already booked someone else." So STL Agent runs on Telegram and pings them in seconds — not because Telegram is trendy, but because it's the one app that already lives on a contractor's lock screen.

I write code with AI agents every day. Not to replace thinking. To multiply the output of one person who already understands the problem deeply. The thinking and the product decisions are mine. The implementation moves at a pace that used to require a team of four. (AI is not a substitute for thinking. It is a multiplier.)

Every product on this page was built this way. From first conversation to live users. Numbers above are real.

## What I believe

Writing code stopped being the bottleneck. Understanding the problem is the only hard part now, and it has always been the only hard part. Knowing what to build, for whom, and why. That hasn't changed in twenty years.

What changed: a single developer who actually talks to the user can now ship the fix the same week. That is the real edge. Not the AI tooling. Not the framework. The willingness to talk to the person, then build the small specific thing that helps them this Tuesday.

### The same kind of hire as search and social

A generation ago, serious local businesses realized they needed someone who understood **search** and someone who understood **social**. The next recurring hire is whoever makes **AI actually run in the business** — not a strategy deck, but faster follow-up, fewer dropped quotes, and money not walking to whoever answered first.

That is what **[STL Agent](/projects/stl-agent/)** is for contractors: I show up as the **speed-to-lead operator** — embedded in Telegram, accountable, built on Rails — for a handful of GTA trades at a time. Same category of relationship as “our SEO person” or “our IT guy,” but for the moment the customer decides who gets the job.

## Get in touch

If you are a Toronto contractor losing jobs to slow follow-up, start with the **[free Speed-to-Lead Score](/stl-score/)** — 30 seconds, no spam, instant number.

If you have a different problem worth solving — civic, SMB, anything where one developer with Rails + AI can build the right tool — [get in touch through the contact page](/contact/). I reply inside an hour, usually less.

Also on [GitHub](https://github.com/AhmedNadar) and [Twitter](https://twitter.com/ahmednadar) — most of my building-in-public happens there.
