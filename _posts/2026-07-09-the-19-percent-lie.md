---
layout: post
title: "The Productivity Tool That Made You Slower"
date: 2026-07-09
description: "AI tools made experienced developers 19% slower while those same developers felt 20% faster. That gap is the whole story."
tags: [ai, developer-productivity, software-engineering]
---

The cursor fills in a whole function faster than you could have typed it, and for one second you feel faster. Anyone who codes with AI knows that second. It is real, it is satisfying, and building Rapidfy and SolveTO I have felt it a hundred times a day. What I learned the hard way is that the feeling and the actual speed are two different numbers, and they rarely match.

METR put a number on the distance between them. They studied 16 experienced developers working in real open-source codebases, repositories averaging 22,000 GitHub stars and over a million lines of code, the kind of system where one wrong abstraction costs you a week. The developers who used AI tools took 19% longer to finish their tasks than the ones who didn't. Afterward, those same developers estimated that AI had sped them up by 20%. Thirty-nine points sit between what happened and what they felt happened, and that gap is the most important fact in the whole argument about AI and developer productivity.

![The 19 percent lie]({{ site.url }}/assets/images/posts/the-19-percent-lie.png)

Most of the industry answered the study by attacking the methodology. The 2026 follow-up made that harder, not easier. METR paid its developers $50 an hour, and many of them still refused to work in the no-AI control condition, skipping a large share of those tasks rather than give up the tools. The developers who stayed willing to work without AI were, by selection, the ones it was helping the least, so the follow-up measured a skewed sample. But the perception gap showed up across the entire dataset, and that is the part nobody has reckoned with.

The reason you feel faster is not a trick your brain plays on you. Autocomplete removes friction at the exact moment friction is most annoying, the empty function, the boilerplate constructor, the fifth identical test setup. You type less, the file fills in, the cursor keeps moving. That sense of forward motion is legitimate, and it costs you something you don't see for three months.

After teams adopt AI, the maintenance bill grows. GitClear's code analysis found duplicated code climbing and refactored code falling as AI use spread, the kind of pattern that turns into technical debt later. The code an AI writes is often correct for the case in front of it and wrong for the system around it. A function that passes its test, works in isolation, and quietly couples two layers that should never have touched does not announce itself. It shows up at 2am six weeks later, when someone edits the file it secretly depended on.

84% of developers now use AI tools, and the productivity their organizations actually gained lands around 10 percent. The math fills in the rest. The speed you win on a single task does not transfer to the system, because the review, the QA, the integration, and the architectural judgment are still yours to do. AI made the easy part faster. The hard part is still the hard part. I made that case before any of this data landed, in [what are you building while AI speeds up](/what-are-you-building-while-ai-speeds-up/).

What actually helps is different from what feels productive, and the line between the two is sharp. Code search that lets me navigate a system I have never seen removes a bottleneck that was genuinely slowing me down. A debugging agent that gathers production context before I start saves time I used to waste by hand. Review tools that catch the mechanical errors free a reviewer to spend attention on the structural ones. Each of those removes a specific friction from a specific place, which is a different move from bolting autocomplete onto everything and counting lines of code. It is also why I push verification outside the agent loop in my own harness, the reasoning I wrote up in [where verification actually belongs](/where-verification-actually-belongs-in-agent-harnesses/) and [context window economics](/context-window-economics-for-ai-agents/).

Claude Code leads for large refactors and reasoning across a big codebase. Cursor leads for daily coding, where holding the context in your head is half the work. Both are strong tools, and both get used in shops where the surrounding process never caught up. There is no better way to measure output quality, no shared standard for what AI-assisted review should catch that a human shouldn't have to. The tools outran the practice.

METR's 2026 update did not flip the result. Real-world developers still came out slower, around 18% by its measure, with error bars wide enough to cross zero and the same self-selection baked in. The tools improved and developers learned to drive them, and the gap may be closing, but the measured number is still a slowdown, not the speedup the industry keeps assuming. Where it lands for you depends on what you are building, how complex the system already is, how good your test coverage was before AI touched it, and whether you trust what comes out.

That last one decides everything. Trust is a calibration problem. The developers who do best with these tools have learned the exact line between output they can accept on sight and output they have to read line by line. That calibration takes real time and a clear view of the failure modes. The ones who struggle either reject everything by reflex or accept everything by default, and neither of those is engineering.

The tools are worth using. The only real question is what you are speeding toward. Move faster through a system you already half-understood and you are not building faster, you are arriving sooner at the moment the misunderstanding turns into a bill someone has to pay.
