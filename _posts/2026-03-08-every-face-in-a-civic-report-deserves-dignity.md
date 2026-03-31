---
layout: post
title: "Every face in a civic report deserves dignity"
date: 2026-03-08
description: "When someone reports a person sleeping rough on a sidewalk, they're asking for help, not publishing a mugshot. How SolveTO handles homelessness reports differently."
tags: [solveto, privacy, civic-tech]
---

Last week a report came in of a person sleeping rough on a Toronto sidewalk. The photo was clear. The face was visible. The report went to 311 and a ward councillor with that face completely exposed.

That's not a bug. That's a failure of care.

When someone reports a pothole, privacy isn't the first thing you think about. But when the issue is a person, someone sleeping on a grate, someone sitting against a building with a sleeping bag, the photo is different. The person in it didn't consent to being photographed. They didn't ask to be in a civic report. They're at the most vulnerable point in their life, and I was publishing their face to city officials.

I couldn't ship another feature until this was fixed.

## The two problems

**Problem one: the AI didn't know what it was looking at.** SolveTO had 30 issue types. Potholes, graffiti, broken streetlights, illegal dumping. Nothing for homelessness. So when someone photographed a person sleeping rough, the AI classified it as "parks issue" or "debris." That's dehumanizing. A person isn't debris. A sleeping bag in a doorway isn't a parks maintenance problem.

**Problem two: face redaction was broken.** I had a face detection system. It was supposed to find faces and redact them before the report was stored or emailed. It had been silently failing since launch. Every report with a visible face went out unredacted.

## What homelessness reports look like now

The 31st issue type is "Homelessness / Encampment." The AI now recognizes sleeping bags on sidewalks, cardboard bedding in doorways, tents on public land, people sleeping on grates. It distinguishes between someone's sleeping spot and illegally dumped trash. That distinction matters.

When a report is classified as homelessness, three things happen that don't happen for a pothole:

**A help banner appears.** Before you see the photo, before the report details, there's an amber bar: "Need help? Toronto Streets to Homes (S2H) (416-338-4766), 311 Shelter Referral, or call 211 for community services." The report exists to connect someone with help, not to complain about them.

**Emails include a sensitivity note.** When the report reaches the city, there's a line at the top: "This report involves a person experiencing homelessness. Please handle with sensitivity and dignity." It's one sentence. It reframes the entire email from "here's a problem to clean up" to "here's a person who needs outreach."

**Face redaction is mandatory.** If face detection fails on a homelessness report, the report doesn't send. It waits for review. A pothole report with a failed face scan still goes out, the stakes are lower. A homelessness report with a visible face stays in draft. No exceptions.

Face detection now works reliably across upright, tilted, sideways, and lying-down faces. The redaction is permanent and irreversible.

## Why this matters

The default in civic reporting is to treat everything the same. An issue is an issue. A form is a form. But a person sleeping on a sidewalk is not the same as a broken streetlight. The reporting interface, the classification, the language, and the privacy handling all need to reflect that.

Jason Fried talks about building software that respects people's time. I'd add: respect their circumstances too. The person submitting the report has good intentions. The person in the photo has no say in the matter. The software has to protect both.

This is the kind of feature that won't show up on a KPI dashboard. It won't increase conversion. It won't reduce churn. But it's the right thing to build.

SolveTO now has 31 issue types covering every category in Toronto's 311 service request catalogue, plus homelessness. Face redaction is automatic and permanent.

If you see someone who needs help in Toronto, you can report it at [solveto.ca](https://solveto.ca). The report reaches the right people. The face stays private. The banner shows the phone numbers that matter.

That's the minimum. Not a feature. A baseline.
