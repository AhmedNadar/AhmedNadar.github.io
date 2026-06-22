---
layout: project
title: SolveTO
description: "Civic accountability across Toronto, Mississauga, and Milton. Over 1 million public assets mapped. AI turns a phone photo into a real 311 report in under 30 seconds."
link: https://solveto.ca
featured: true
order: 1
status: "Live in 3 cities"
---

SolveTO is a civic accountability platform now live across three cities: Toronto, Mississauga (SolveSAUGA), and Milton (SolveMILTON). One account, one login, one report history that follows you across city lines. Any resident can report any issue in under 30 seconds. I talked to residents, understood what was frustrating about reporting local issues, designed the flow, and built the whole thing with Rails and AI agents. It is live and serving real users.

<figure class="project-showcase project-showcase--single">
  <img src="{{ '/assets/images/posts/og-toronto-deserves-better-than-311.png' | relative_url }}" alt="SolveTO map of Toronto connecting civic reporting with open data" width="1200" height="630" loading="lazy" decoding="async">
  <figcaption>Community map, wards, and accountability — built on Toronto open data.</figcaption>
</figure>

## The Problem

Toronto's 311 system handles 600+ service types but costs the city **$11 to $16 per phone call**, and fewer than 20% of residents use the app. The existing app assumes you already know how city departments work, which category your issue falls under, and how to write a formal complaint. Most people notice problems every day and never report them because it is too annoying.

## How It Works

The workflow is three steps. **Spot** an issue and snap a photo. **Send** it through SolveTo where AI analyzes the image and generates a proper report. The city's **Fix** teams get notified with everything they need to act. Reports are also posted publicly on a community map so neighbors can see what is happening and verify issues.

## What Makes It Different

Community verification lets neighbors vote on reports, building trust through crowd validation. The platform now maps over 1 million public assets across Toronto, Mississauga, and Milton, all from public open data, with wards and boundaries fully integrated so you can view issues by area and get local statistics. Every report gets detailed resolution tracking with status updates and before/after documentation. A public analytics dashboard tracks councillor performance, SLA compliance, and community engagement. The hard part was never one more city. It was building one platform that does not start over every time a resident moves or a new town comes online.
