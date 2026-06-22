---
layout: post
title: "Two product decisions that respect the person on the other side of a report"
date: 2026-04-04
description: "Protecting the dignity of every face in a civic report, and removing the friction that stops people from reporting at all. Both are about respecting the human behind the report."
tags: [solveto, privacy, civic-tech]
---

I think about civic reporting as a relationship between two people who never meet. One person sees something wrong on the street and decides to say something. Another person, somewhere in the photo, never asked to be part of it. Most reporting tools forget that both of them exist. I built two decisions into SolveTO that start from the opposite assumption, that both deserve respect, and over time I realized they were the same decision wearing two faces.

<!-- IMAGE: Split frame, a blurred face shielded in a report on one side, the one-tap Snap voice flow on the other. Add to /assets/images/posts/ and reference as {{ site.url }}/assets/images/posts/dignity-and-friction.png -->

The first one came from a report I will not forget. Someone flagged a person sleeping rough on a sidewalk. The photo was clear, the face was visible, and it went out to the city and a ward councillor completely exposed. The person submitting it had good intentions. They were asking for help, not publishing a portrait of someone at the lowest point of their life. But the software treated that face like it treated a pothole, as just another pixel to pass along, and that is a failure of care, not a feature gap.

When the issue is a person, the photo is different from every other kind of report. The person in it did not consent to being photographed and did not ask to be in a civic record. So I made [face protection automatic](https://solveto.ca/blog/every-face-deserves-dignity). Every face that ends up in a report, the subject of it and any bystander who happened to be in frame, gets shielded before that report is stored or sent anywhere. Nobody is exposed to a city official, a councillor, or anyone else. And when the issue is someone experiencing homelessness, the standard is stricter still. If a face cannot be protected, the report does not go out. It waits. There is no version of "close enough" when [a person's dignity is the thing at stake](/every-face-in-a-civic-report-deserves-dignity/).

That is not the kind of work that shows up on a dashboard. It will not increase a single number anyone tracks. But it is the baseline I am unwilling to ship without, because the person submitting the report trusted the system to handle it well, and the person in the photo had no say at all. The software has to protect both of them. That is the floor, not the ceiling.

The second decision looks like the opposite problem and turns out to be the same one. My wife spotted a pothole while driving and took a photo through the windshield, blurry, at an angle, from a moving car. It failed. And the lesson was not that she did it wrong. The lesson was that the process asked too much of her. Spot the issue, pull over, get out, frame a clear photo, open the app, fill out a form with location and category and description, then submit. Seven steps for a pothole. No wonder almost nobody does it.

People are not apathetic about their city. They are busy. The form takes five minutes, most people do not have five minutes, so nothing gets reported, and [the city fixes what gets reported](/week-one-in-the-open/). You can see where that goes. The bottleneck was never that residents do not care. It was that caring cost more than it was worth, every single time.

So I [built Snap to remove the cost](https://solveto.ca/blog/why-snap-exists). You tap once, you speak a sentence like "big pothole on Don Mills near the gas station," and you are done in about ten seconds. No photo to frame, no form to fill, no map to scroll, no waiting on a spinner while you sit at a green light. Your voice carries everything the city actually needs, the what and the where, and the rest happens after you have already put your phone down and driven on. When several people flag the same spot, those voices stack into something a single report never could, real evidence from the people who drive those roads every day. None of this replaces 311. It feeds the same crews the same information, just through a door that opens.

What ties the two together is the question I now ask before building anything. Who is on the other side of this report, and does the design respect them. The face redaction respects the person who cannot speak for themselves in the photo. Snap respects the person who wants to help but has ten seconds, not five minutes. One protects someone from the system, the other invites someone into it, and both refuse to treat a human being as just another input to process.

I am building this myself, at night, instead of waiting for the city to get to it, because these are the decisions that get cut first when you optimize for metrics. A face stays private not because it moves a number but because it is a person. A report takes ten seconds not because speed is impressive but because friction is what kept good people silent for years. Respect the human on the other side, and the rest of the product follows from there.
