---
layout: project
title: SolveTO
description: "Civic accountability for Toronto. 418,000 city assets mapped. AI turns a photo into a real 311 report in 10 seconds."
link: https://solveto.ca
featured: true
order: 1
status: "Live"
---

SolveTO is a civic accountability platform for Toronto. Any resident can report any issue in 10 seconds. I talked to residents, understood what was frustrating about reporting local issues, designed the flow, and built the whole thing with Rails and AI agents. It is live and serving real users.

<figure class="project-showcase project-showcase--single">
  <img src="{{ '/assets/images/posts/og-toronto-deserves-better-than-311.png' | relative_url }}" alt="SolveTO map of Toronto connecting civic reporting with open data" width="1200" height="630" loading="lazy" decoding="async">
  <figcaption>Community map, wards, and accountability — built on Toronto open data.</figcaption>
</figure>

## The Problem

Toronto's 311 system handles 600+ service types but costs the city **$11 to $16 per phone call**, and fewer than 20% of residents use the app. The existing app assumes you already know how city departments work, which category your issue falls under, and how to write a formal complaint. Most people notice problems every day and never report them because it is too annoying.

## How It Works

The workflow is three steps. **Spot** an issue and snap a photo. **Send** it through SolveTo where AI analyzes the image and generates a proper report. The city's **Fix** teams get notified with everything they need to act. Reports are also posted publicly on a community map so neighbors can see what is happening and verify issues.

## What Makes It Different

Community verification lets neighbors vote on reports, building trust through crowd validation. Toronto's 25 municipal wards are fully integrated. You can view issues by ward, see ward boundaries on the map, and get ward specific statistics. Every report gets detailed resolution tracking with status updates and before/after documentation. A public analytics dashboard tracks councillor performance, SLA compliance, and community engagement across all 25 wards.
