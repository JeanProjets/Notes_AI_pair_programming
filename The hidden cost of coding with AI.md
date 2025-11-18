The hidden cost of coding with AI: overconfidence, overengineering… and wasted time
Since I started coding with AI, I’ve noticed two sneaky traps that end up costing me a lot of time and mental energy.

The “optimal architecture” trap The AI suggests a clean, well-structured pattern. It looks solid, better than what I would’ve written myself, so I go with it. Even if I don’t fully understand it. A few days later, I’m struggling to debug. I can’t trace back the logic, I don’t know why it broke, and I can’t explain what’s going on. Eventually, I just revert everything because the code no longer makes sense.

The “let’s do it properly now” spiral I just want to call an API for a small feature. But instead of coding only what I need, I think, “Let’s do it right from the start.” So I model every resource, every endpoint, build a clean structure for future-proofing… and lose two days. The feature I needed? Still not shipped.

Am I the only one? Has anyone else been falling into these traps since using AI tools? How do you avoid overengineering without feeling like you’re building something sloppy?


These are near universal truths about software development, and have been true since almost the dawn of programming 50+ years ago. The only thing that is different because of AI, is the speed at which you encounter these same truths (before, it used to take weeks, months, or years to realize these kinds of mistakes).

An "optimal architecture" that some old school software architect drafted up, but nobody else actually understands, leading to code that nobody really groks later on? Or same idea, but with a paid consultant or a key employee providing this "optimal architecture" but then leaving the company, and nobody else remembers why it was done that way? Tale as old as time...

Or, just plain old YAGNI -- there's a reason why that acronym (you ain't going to need it) was created and became a cliche in the first place... Software engineers have been naively building more abstraction than they really need since the dawn of time.

Really what these kinds of realizations show to me, is that Claude Code truly has found and landed smack dab in the middle of what software engineering actually is... In that sense, this is proof that Anthropic nailed what SWE is all about, because rediscovering these age old lessons of the trade, means that Claude Code is not just a fad, and not such a different paradigm that engineers can't apply the standard wisdom of their craft.

And for your benefit, the standard wisdom of the ages for dealing with these issues is to not let your architecture get too far ahead of your actual use cases, and to avoid premature abstraction (as just another example of a type of premature optimization evil). Build what you need, actually use it, actually understand how it works, then wait until you have to duplicate yourself for a second or third time, before you actually create a new abstraction (in other words, letting the duplication exist and paying the cost of doing something twice or thrice, is a reasonable price to pay for validating that something is actually worth abstracting AND that you truly understand the abstraction itself, in order to save yourself the risk of YAGNI and building something too early before you understand it and thus throw it away later).