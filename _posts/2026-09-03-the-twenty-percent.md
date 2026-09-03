---
layout: post
title: "The Twenty Percent"
date: 2026-09-03
description: "Steve Jobs told a room at MIT in 1992 that his developers wrote twenty percent of the code. Everyone remembers the speed. Twice since then I have watched the same shift arrive and get called a toy."
tags: [business, ai, building software, solo-founder, lessons]
published: true
---

In the spring of 1992 Steve Jobs stood in front of a room at [MIT Sloan](https://mitsloan.mit.edu/ideas-made-to-matter/steve-jobs-talks-consultants-hiring-and-leaving-apple-unearthed-1992-talk) and said something that sounded like bragging about speed. On NeXTSTEP, he told them, "you write about 20% of the code that you do in any other development environment we know of." Apps got built "five to 10 times faster." Then he gave the reason, and the reason is the part I care about: "the code that's the fastest to write, the code that's the easiest to maintain, and the code that never breaks, is the code you don't write."

He was talking about objects. Objective-C, reusable and pre-configured components shipped in the box. Most of any application already existed as something somebody else had written and tested, so his developers were not writing eighty percent of their programs. They were putting them together, and spending their real effort on the twenty percent that made the product theirs.

He was [selling](https://singjupost.com/transcript-steve-jobs-speaks-at-mit-sloan-distinguished-speaker-series/) to Fortune 500 IT departments, and everyone in that room, him included, still believed you had to be big to get in the door. Then one moment gets away from him. A breakthrough spreadsheet, he says, would need fifty million dollars just to rise above the noise, so the brightest people he knows are not building apps at all. They are writing objects for other developers to build on, and they are "going where everybody isn't." That is a plan for people with no money and nobody's permission, said out loud by the man selling to the Fortune 500.

So here is my claim, and it is mine, not a quote I am handing him. If most of the program already exists and is already tested, the thing that shrinks is not your typing. It is how many people you need before you can start. He sold objects as a way for big companies to move like small ones. What he had found was a way for small ones to try what only big ones were allowed to try at all.

## Both chairs

Eighteen years after that talk I was in a room where the same argument was made to my face, and I was the one holding the toy. I had built on Microsoft's stack for years, C# and .NET, with Rails on the side at night because it was fun. Around 2010, with [Rails on version 3](https://railsapps.github.io/rails-release-history.html), I was at a Microsoft conference in Toronto with friends who were senior developers and VPs of engineering, and I asked whether leaving the stack for Ruby was a good idea. Oh boy, I have rarely watched a room turn that fast.

Ruby was not a real language. Rails was not a real framework. One of them told the truth without meaning to. If a language is that easy, he said, then anyone can use it, and if anyone can use it, what separates him from real developers? He had years in C, C++, C#, Java, and that was the investment. Someone else put it flatter. Those are toys, that is Lego, real software comes out of Java and C.

He was right about the Lego. I wanted to move because it was fun to write less code. Ruby is fun to write because it holds things in the language that I was building by hand in C#, and Rails already contained decisions I used to make one file at a time and then defend in review. I was not smarter in Ruby. I was snapping more pieces together with joy and shipping in an evening what used to take me a week, and I had stopped apologizing for the pieces.

That was Jobs's twenty percent, eighteen years later, getting the reaction that idea always gets from the people who paid full price for the old way. Ruby's design goal, the thing [Rails still puts first in its doctrine](https://rubyonrails.org/doctrine), is programmer happiness, and who doesn't like a language that makes them happy? That was the part the room could not forgive. Nobody in there hated a syntax. They had built who they were out of how hard the work was, and I was describing a thing that made it easy.

Three years before that conference I had been sitting in the other chair. When Facebook opened past the colleges and Twitter showed up, I said what everyone around me said. It's a fad. It's for kids. Real businesses don't live there. [Steve Ballmer](https://techcrunch.com/2007/10/02/fadnation-why-steve-ballmer-could-be-right/) said the same thing about Facebook's valuation, weeks before Microsoft bought into it. The people I wrote off as gurus were not more talented than the ones who sat it out. They were allowed to try, because nobody had thought to guard a door that did not exist yet.

## Thirty-four years later

I run a one person software company now. AI writes most of my code, some days nearly all of it, which makes my job title something like senior reviewer of a very fast junior who never sleeps and never learns. What objects were to Jobs, models are to me, and I do not write the eighty percent. I direct it. Objects let you reuse code. Models let you reuse skill, so I can work in fields I never trained in. He got to reuse his own past. I get to reuse everyone's.

Now the honest part, because posts that cheer for AI go bad fast. The eighty percent claim is strong for writing code and much weaker for trusting it. What kills a small software company was never typing. It is features quietly breaking each other, the thing that worked on Tuesday failing on Friday for a reason nobody wrote down. AI moved the hard part from writing the code to checking it, so I spend less time making software than I ever have and more time proving it than I expected. The objects were worth something because they were already tested. Reusing them was about trust before it was ever about speed.

## The floor

What is the comfortable answer for why big companies do not take an advantage this large? They are slow and stupid, but Apple's engineers in 1995 were not stupid and [Copland](https://lowendmac.com/2005/apples-copland-project/) still died with hundreds of engineers on it. Two years later Apple paid $429 million for NeXT, for the software it had ignored, and every iPhone in the world runs what grew out of it. The answer sits in the org chart. Headcount is the budget and the status, so a tool that makes eight engineers unnecessary is a threat to the manager of those eight engineers, whatever the slide deck says. The people who have to approve the change are the people the change costs.

There is a second rule underneath it, and it shapes what I build. A big company cannot afford to do something small. Getting anything approved, staffed, bought, and cleared by the lawyers costs about the same whether the project is big or tiny, so every institution has a floor, and below that floor no problem is worth solving no matter how much it matters to the people who have it. I have written about this as the city [playing golf with one club](https://solveto.ca/blog/one-club-i-brought-fourteen). The floor is why the bag never opens.

Everybody has a floor, not just city hall. A ten person shop has one, lower down. A team of two has a lower one still. A person working alone has the lowest floor of anyone, and that is the whole advantage. It is not speed. It is the size of the problem you are allowed to take seriously.

![Above the floor: $4M and up, two years, RFP, vendor, committee. Below it, the small local problems nobody is funded to solve, and one that got built anyway.]({{ site.url }}/assets/images/posts/the-twenty-percent.png)

I ran into that floor in one conversation recently. I build [civic software in Canada](https://solveto.ca/blog/toronto-311-plan-2027-i-shipped-it-in-february), the broken and ordinary problems no pitch deck mentions. When I compared notes with someone who works inside government, the answer was not that it was a bad idea. It was that roughly half of what I had built alone sat on their roadmap for 2028, and buying it through their process would cost four to six million dollars.

Two years and millions of dollars, not to invent anything. To buy permission to start. The endless cycle of paperwork, the vendor, the committee, the budget year, the political cover. The software was never the expensive part. One person with today's tools built it under their floor, in less time than their process takes to book its first meeting.

## What is rare now

Rory Sutherland keeps recommending a 1916 story called [Obvious Adams](https://archive.org/details/obviousadamsstor00upderich), about a man who does well in advertising by noticing what is sitting in plain sight while cleverer people look past it. The obvious thing in 2026 is that the technology is not the bottleneck. [I have said that about the city](/obsession-or-stubbornness/) and it is true here too.

Sutherland likes to say the opposite of a good idea can be another good idea, so here is the flip I keep. For seventy years software companies won by writing code other people could not write. Now that everyone can write everything, the rare things are the ones no engineering budget ever had a line for: taste, warmth, and trust. The rare skill is knowing which small thing matters, being free to go do it before a committee could meet, and being willing to build what will never pay for itself on a spreadsheet.

My other product lives on a grandmother's Mac, the most powerful machine in her house and a very expensive way to click on things. She does not need it smarter. She needs it to remember her, her family, her mornings, and to keep those memories on her own disk and nowhere else. No funded company would build that. The market is too small and you cannot sell data you refuse to hold, which is the entire reason it is still there to build.

So I spend my eighty percent on the features that only make sense if you are building for one grandmother instead of a billion strangers. AI made it cheap to care about things too small for anyone big to care about. One of my apps sits under the industry's floor. The other sits under the city's. Both are toys, by the standard of that room in 2010, and NeXT looked like a failure for years too. Nobody sitting at MIT in 1992 could tell which one they were watching.
