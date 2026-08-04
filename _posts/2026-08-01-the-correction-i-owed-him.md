---
layout: post
title: "The Correction I Owed Him"
date: 2026-08-01
description: "A former mayor credited me on air with something the city built, and I said yes. What it cost to take that back, and why the cheapest correction is always the one you make earliest."
tags: [civic-tech, leadership, engineering, solveto]
---

Eight minutes into a radio segment on Newstalk 1010, John Tory asked me whether the ability to attach a photo to a 311 request was my work. He ran Toronto from 2014 to 2023 and now covers municipal affairs for the station, and he was being generous with a developer he had just met. I said yes.

That was wrong, and I knew it was wrong before the segment ended.

The City of Toronto built photo attachment. It shipped with the 311 mobile app in January 2022, alongside a rebuilt 311 web experience the previous November, and it was announced by Tory himself while he was still mayor. His own administration did it, years before I wrote a line of my platform. He had forgotten, which is a reasonable thing to forget about a nine-year term. I had not forgotten. I let it stand because the question arrived wrapped in a compliment and the yes was already halfway out of my mouth.

I build a platform whose entire argument is that residents deserve an accurate public record of what happened to their report. Somebody making that argument for a living does not get to accept credit for someone else's work, however kindly it is offered. So I published the correction, in full, with the dates and the attribution, on the company blog where anyone looking for the claim would find the retraction attached to it.

The interesting part is not the ethics. The ethics were never in question. The interesting part is the mechanism that let it happen, because I have spent the last year building systems designed to catch exactly that class of error.

An agent working through a long task will accept a premise handed to it inside a well-formed question and carry that premise forward for fifteen more steps, never revisiting it, because nothing in [the loop](/7-patterns-for-long-running-agent-harnesses/) was built to challenge an input that arrived looking correct. That is the failure I designed Rapidfy's verification layer against, and it is [where I have argued verification belongs](/where-verification-actually-belongs-in-agent-harnesses/): before the step executes, not after the output looks wrong. The check costs ten seconds. Skipping it costs forty minutes of confident work in the wrong direction.

On air I was the agent without the gate. A premise arrived, it was flattering, it was fast, and I passed it through.

What made the correction cheap was catching it inside a day. The claim had been broadcast once, it had not been repeated, nobody had built anything on top of it, and no other outlet had picked it up and made it a fact. A retraction published in that window costs a paragraph. The same retraction three months later, after it has been quoted back at me in two interviews and appeared in someone's write-up, costs credibility I could not buy back with any number of paragraphs. Errors do not stay the same size. They accrue interest in whatever direction the record travels.

There is a version of this that goes the other way, which is the one I keep watching people choose. You let the generous line stand because correcting it makes the segment awkward, and then you let it stand a second time because now you would be correcting yourself twice, and by the fourth time it is load-bearing. Nobody decided to build a false record. It accumulated, one small silence at a time, and each silence was cheaper than the correction until suddenly it was not.

The engineering habit and the civic habit turn out to be the same habit. Check the premise before you act on it. Pay the correction while it is still small. Put the fix in the same place the claim was made, so anyone who finds one finds the other.

<!-- IMAGE: radio studio microphone, or a screenshot of the published correction with the date visible. Add to /assets/images/posts/ and reference as {{ site.url }}/assets/images/posts/the-correction-i-owed-him.png -->

Tory was right about the thing that mattered, and he named it before I had said a word: you cannot track the outcome of your complaint, and the city does not yet have the technology in place to tell you. That gap is real, [Toronto's Auditor General named it in 2011](https://solveto.ca/blog/the-timeline), and it is the one I built for. I would rather be correct about the small thing so the large thing carries weight.

A record you are willing to correct in public is the only kind anyone should trust, and that applies to the one my platform keeps for a resident exactly as much as it applies to the one I keep for myself.
