---
layout: post
title: "Nobody Wanted a Referee. They Needed a Score."
date: 2026-08-02
description: "The golf handicap is one of the oldest fairness inventions in sport and it never judges anyone. Why the products that get adopted keep score instead of keeping verdicts."
tags: [product, business, civic-tech, decisions, measurement]
---

The golf handicap does not judge your swing. It does not rank you, coach you, or tell the club what it thinks of your game. It keeps score, consistently, in public, and that one narrow job is enough to let a weekend player and a scratch golfer compete on the same afternoon without either of them arguing about who is better. The handicap never referees the game. It makes the game refereeable, by putting the number where both players can see it.

That distinction is the most useful thing I have learned building software for an industry that runs on impressions.

A contractor bidding on a road resurfacing job is, more often than anyone in that business likes to admit, bidding on a guess. The tender says how many lane kilometres. It rarely says how many of those kilometres have had three complaints filed against them in the past year versus zero. So the contractor drives the route, or trusts a hunch, or prices in a margin for the parts they cannot see. None of that is incompetence. It is what you do when the only available inputs are observed rather than measured, and it is the same blindness I wrote about when I worked out [what a missed phone call costs a contractor over a year](/why-contractors-lose-50k-a-year-answering-the-phone-too-late/). The money leaks through the gap where nobody was counting.

A business improvement area asking Council for streetscape money is making the same bet from the other side of the table. "Residents keep complaining about our street" is true, and it is unfalsifiable in the way that makes a councillor's staff nod politely and move to the next item on the agenda. A photo and a story were the only tools available, so a photo and a story is what got brought.

Both of those groups are making real financial decisions on evidence that would not survive ten seconds of scrutiny anywhere else in their business. Nobody designed it that way. The measurement never existed, so the decisions grew around its absence, and after enough years the absence stopped looking like a gap and started looking like the nature of the work.

What changes when the number exists is smaller and stranger than I expected. None of the underlying data is new, which is the part that still bothers me. [It was always there](/the-data-was-always-there/), published, free, sitting in a portal nobody had connected to anything. A public, ward-level record of how often something gets reported and how long it takes to resolve does not replace the site visit. It gives the site visit a starting point that is not a hunch. The contractor still drives the route. They drive it knowing which three blocks generate the demand. The BIA still stands at the podium. They stand there able to name the report count for that block and how long the city took to confirm the work, which turns an impression into something a staffer can check.

Neither of those is a verdict. That is the entire point, and it is the part I got wrong in my own head for the first few months.

The instinct when you build measurement into a system that never had it is to build the referee. You have the data, you can see who is slow, and the product that grades performance feels like the honest one. It is also the product nobody adopts, because the moment your software renders a judgment, every party in the system starts arguing with the judgment instead of using the data. You have handed them a fight rather than a tool, and fights are expensive to be inside of.

![The referee renders a verdict and nobody adopts it; the scoreboard keeps score and the argument gets a starting point]({{ site.url }}/assets/images/posts/nobody-wanted-a-referee.png)

The honest version of the civic story matters here too. Most of what gets reported in Toronto and fixed lands inside the city's own published windows, the four-day pothole standard among them. That is not a story of a broken city, and I have never told it as one. The one place I do push is on the word itself, because [a status is not a fact](https://solveto.ca/blog/completed-does-not-mean-fixed) and a ticket handed to another division should not count as a thing that got fixed. That is a scoring question, not a verdict about anyone's effort. The crews were always doing the work. What was missing was not effort, it was a scoreboard both sides could see, and the reason it was missing is that keeping score in public is a commitment nobody had signed up for.

This generalizes past cities, and it is the thing I would tell anyone building into an industry that has run on judgment calls for a hundred years. You are not there to tell people they have been doing it wrong. They know the parts they cannot see, better than you do, and they have been compensating for them longer than your company has existed. What you can hand them is the number they never had, in a form they can check, so the argument they were already having stops being about whose impression is right.

The handicap worked because it was so limited. It answered one question, the same way, for everyone, and it left the game alone. Every durable measurement system I can think of made that same trade, and every product I have watched get rejected by an industry it was trying to help made the opposite one.

Keep score, and people bring you their decisions. Keep verdicts, and they bring you their lawyers.
