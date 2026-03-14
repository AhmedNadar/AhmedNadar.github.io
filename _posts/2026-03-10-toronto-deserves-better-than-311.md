---
layout: post
title: "Toronto deserves better than 311"
date: 2026-03-10
description: "Reporting a pothole shouldn't take 15 minutes on hold. SolveTO replaces 311 with a 30-second photo-to-report flow that costs the city nothing."
tags: [solveto, toronto, civic-tech, ai]
image: /assets/images/posts/og-toronto-deserves-better-than-311.png
---

Three months ago I watched a woman on Bloor Street step around the same pothole I reported by email three weeks earlier. She looked at it, shook her head, and kept walking. She's not going to call 311. Nobody is.

I know because I tried. I called 311 to report a cracked sidewalk near my neighbourhood. Fifteen minutes on hold. Then I described the location verbally to an operator who typed it into a form I couldn't see. No photo. No GPS coordinates. No confirmation it went to the right department. I hung up and thought: that's the last time I'm doing this.

I tried the city's online reporting tool. Six minutes to fill out forms, describe the location in text, select from dropdowns that didn't quite match my issue, and upload a photo through a clunky interface. Six minutes for a pothole.

Most people won't spend six minutes. Most people won't spend one minute. So the pothole stays. The graffiti stays. The broken streetlight stays. Not because nobody cares. Because the process punishes you for caring.

This is why I built SolveTO.

## The math behind the problem

Toronto received 6,839 pothole reports in February 2026 alone. A fivefold increase from last year. The city repaired 257,000 potholes in 2025. The scale is massive.

But here's the number that matters: each 311 phone report costs the city $12 to $15 in call center staff time. That's operator wages, phone infrastructure, data entry, and ticket routing. Multiply that by hundreds of thousands of calls per year.

In the UK, Buckinghamshire Council measured the exact same problem. Email-based reports cost them $12.50 each. When they switched to FixMyStreet, an online civic reporting platform, that dropped to $0.15. The City of Melbourne in Australia saw a 17% improvement in citizen satisfaction scores after adopting Snap Send Solve, a similar platform.

These aren't hypothetical gains. Cities worldwide are replacing phone-based reporting with digital platforms and seeing real results. Toronto is behind.

## What I built

[SolveTO](https://solveto.ca?utm_source=toronto-deserves-better&utm_medium=blog) is a free civic reporting platform for Toronto. It's been live since early February 2026, covering all 25 wards. It costs the city nothing.

Here's how it works.

You open solveto.ca on your phone or desktop, take a photo of the issue and submit it. That's it. AI identifies what it is, pothole, graffiti, broken streetlight, illegal dumping, one of 24 categories. It rates the severity. It writes a structured report. GPS pins your exact location and maps it to the correct ward and postal code. The report goes to 311@toronto.ca and your ward councillor simultaneously.

**Thirty seconds. No phone tree. No forms. No hold music.** It's that simple.

The report appears on a public live map that anyone can see. Other residents can verify the issue exists. When it's fixed, anyone can submit a resolution photo. Everything is tracked, transparent, and public.

## What the live map shows you

The homepage is a full-screen interactive map of Toronto with every report plotted in real time. It refreshes every five minutes.

You can filter by any of the 24 issue categories, each showing a live count. Filter by time: last 24 hours, 48 hours, 7 days, 30 days. Filter by ward, click any of the 25 wards to see its boundary drawn on the map and all data filtered to that ward. Toggle between open and closed reports, each showing how many exist.

Every filter combination is encoded in the URL. So you can bookmark "all open potholes in Ward 11 from the last 7 days" and share that link with your councillor. The charts update live as you filter. The sidebar updates. The counts update. Everything is connected.

This isn't a static dashboard. It's a live accountability tool.

## What councillors and the city get

For councillors, SolveTO provides something that doesn't exist today: a real-time view of what's broken in their ward.

Ward report cards show total reports, open vs resolved, resolution rate, average response time, and top issue types. There's a downloadable PDF for council meetings and budget requests. Councillor effectiveness is publicly visible, ranked by responsiveness and resolution rate.

For city operations, every report arrives structured and categorized. AI identifies the responsible department, Transportation, Parks, Solid Waste, Toronto Hydro, Municipal Licensing, or Parking, so reports can be triaged instantly. Reports include photos, GPS coordinates, severity ratings, and safety risk flags. No phone call. No manual data entry.

Duplicate detection clusters reports at the same location within 50 metres. If three people report the same pothole, the city sees one cluster, not three separate work orders.

## The features that matter

**Camera-first reporting.** Take a [photo](https://solveto.ca/report) and AI from [anthropic](https://www.anthropic.com/?utm_source=toronto-deserves-better&utm_medium=blog) handles the rest. No typing required unless you want to add description. Works on any phone with a browser. No app to download.

**24 issue categories.** Potholes, graffiti, illegal dumping, illegal signage, streetlights, tree issues, snow and ice, sidewalk damage, abandoned vehicles, parking violations, and 15 more. AI picks the right one.

**Face redaction.** AI automatically detects and blurs faces in photos before they're stored or delivered. Bystander privacy is protected by default, not as an afterthought.

**Duplicate detection.** If someone already reported the same issue within 50 metres in the last 30 days, you'll see it before submitting. This prevents redundant 311 tickets and helps prioritize repeat complaints.

**Community verification.** Neighbours can confirm a report: "confirmed," "still here," or "fixed." Democratic validation, not just individual complaints.

**Outside-Toronto alerts.** If your GPS falls outside city limits, you're warned before submitting. No wasted reports.

**Public analytics.** Open at [solveto.ca/analytics](https://solveto.ca/analytics). KPIs, resolution rates, response times, SLA compliance. Ward leaderboards. Councillor effectiveness scores. All public.

**Works everywhere.** Desktop and mobile. Add it to your home screen and it works like a native app. No app store needed. Works offline too, your report saves locally and syncs when you're back online.

## What's next

SolveTO is live and serving Toronto residents today. Anyone can report an issue right now and it reaches 311 and their ward councillor for FREE. That works. But it's only half the picture.

Platforms like [FixMyStreet Pro](https://www.fixmystreet.com/) in the UK and [Snap Send Solve](https://www.snapsendsolve.com/) in Australia started the same way: citizens reporting, cities receiving. Then cities partnered with them. FixMyStreet Pro now serves 30+ UK councils. Snap Send Solve processes reports for 850+ organizations across Australia and New Zealand. Both operate as city partners, not volunteers. The city contracts the platform, gets direct integration with its internal systems, and the reporting pipeline becomes part of city operations.

That's the model. SolveTO works on its own, but without city partnership it hits a ceiling. Reports go to 311 by email. We can't connect directly to the city's work order systems. We can't close the loop and tell residents when their pothole is actually fixed. We can't route reports straight into departmental queues. The platform is ready for all of that. The city just has to say yes.

If you're a resident, use it. Report what's broken. The more reports that land on city desks through SolveTO, the harder it becomes to ignore.

If you're in city operations or on council, let's talk. This isn't a side project. It's infrastructure that already works, built to integrate with yours.

[solveto.ca](https://solveto.ca?utm_source=toronto-deserves-better&utm_medium=blog) — thirty seconds, that's all it takes.

## By the numbers

| What | Number |
|------|--------|
| Issue categories | 24 |
| Toronto wards covered | 25 |
| Time to report | ~30 seconds |
| AI analysis time | ~5 seconds |
| Delivery per report | 311 + ward councillor |
| Duplicate detection radius | 50 metres |

---

If you live in Toronto, use [SolveTO](https://solveto.ca?utm_source=toronto-deserves-better&utm_medium=blog). Report what you see. Report it again when it's not fixed. Share it with your neighbours. Tell your councillor it exists. Bring it up at community meetings. The more residents who report, the more data the city can't ignore.

If you work for the City of Toronto or any other Canadian city, SolveTO is ready to integrate with your systems today. I'm open for business. [Get in touch](https://ahmednadar.com/contact/).