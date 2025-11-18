Junior devs can't work with AI-generated code. Is this the new skill gap?
Question
We explicitly allow and even encourage AI during our technical interviews when hiring junior developers. We want to see how candidates actually work with these tools.

The task we provided: build a simple job scheduler that orchestrates data syncs from 2 CRMs. One hour time limit with a clear requirements breakdown. We weren't looking for perfect answers or even a working solution but wanted to see how they approach the problem.

What I'm seeing from recent grads (sample of 6 so far):

They'll paste the entire problem into Claude Code, get a semi-working codebase back, then completely freeze when asked to fix a bug or explain a design choice. They attempt to fix the code with prompts like "refactor the code" or "fix the scheduling sync" without providing Claude with useful context.

The most peculiar thing I find is that they'll spend 15 mins re-reading the requirements 3-4 times instead of just asking the AI to explain it.


Useful-Emergency-783
•
17d ago
•
Edited 17d ago
Profile Badge for the Achievement Top 1% Commenter Top 1% Commenter
Reading code is always very hard and often much harder than building, and nobody wants to read other people's code unless they want to learn or get paid to do so. The seniors are the ones who read code much better than others, even though a lot of PR reviews are just a quick LGTM. With AI-generated code, you need to review/read even more code in an even shorter period of time because it generates very fast. So it's easy to see why juniors will struggle with this.

Also, in my experience, as a founding member of a startup that builds and delivers so many features every day, the literal limit why my team and I cannot go faster is that we can't read and review AI code fast enough.


Lucidaeus
•
17d ago
"They attempt to fix the code with prompts like "refactor the code" or "fix the scheduling sync" without providing Claude with useful context."

And this is the biggest flaw I'm observing from people using AI tools as well. They don't seem to understand the importance of their prompts and how that is what determines the result. That said, I've observed the same with people using Google... it's... impressive how bad people are with translating their thoughts and emotions into another format, lol.


Limp_Technology2497
•
17d ago
IMHO, it's a usage gap in not knowing how to use the tools properly. They need to know how to use ask/plan type modes to come up with a game plan for solving these sorts of problems, and then break up the implementation, identify a testing strategy, set up a step by step plan, etc.

And, you generally want to avoid long, iterative conversations, instead seeking to have your specification such that the proper solution (or part of the solution, as you're performing multiple steps) can be produced in one shot.

My experience has been that while coding has become somewhat less important with these tools, software engineering best practices are more important now than ever.


almostsweet
•
17d ago
•
Edited 16d ago
This is probably going to be an unpopular opinion in this particular subreddit.....

I can debug a program in my head without stepping through a debugger and tell you what is wrong with it, even complex threads; any language - perl, c#, java, python, kotlin, go, C++, etc. But, I have decades of experience. But, I've been able to do that since high school. I've always LOVED coding. You have to love it and really enjoy it and a lot of people don't get into software engineering for love but because they see money. The young devs who were bad back in my day were bad for the same reasons as the ones today are, they didn't actually care. Anyway, I digress.

Here's the unpopular opinion...

The danger of AI is that it becomes a crutch not just the junior devs but also the seniors. When you're handed a magic box that just spits out an almost complete program it feels like magic, it's pretty exhilarating to see it come to life exactly the way you imagined. It's that hit you get when you succeed in writing a program yourself but faster and more frequent.

However, it atrophies your mind, you become complacent and used to relying on it. This is more of a problem for junior devs who don't see a point in trying harder or doing it themselves, so they never learn the skills they need to understand and care about what the code is doing. If you rely on a GPS to get you where you're going you never establish the neurons to remember how to get there yourself without the GPS. And, likewise, if you never establish the neurons needed to solve the really hard coding problems or navigate the code in your mind and truly understand what it is doing then you're going to suddenly be helpless when we take the llm away.

And, in fact I've seen this even with debuggers, in college I had a contest with another engineer who claimed he was better than me; he relied heavily on debuggers to step him through the code whereas I understood the flow and knew what was in the variables in my mind. We both were helping two guys with the same lab and I ended up solving the problem in minutes for my labmate and he was still there an hour later and still nowhere near the solution. He took the long way towards solving the problem because he had to rely on the debugger and couldn't think of the program any other way. I was able to shortcut across swaths of flow and threads with my mind and get the solution right away. While he sat there freezing threads and stepping and trying to make sense of it all. And, that was JUST a debugger, and it was already a crutch for him. I do use debuggers these days, but I'm glad I was against them when I was younger because it forced me to comprehend the code on a deeper level.

For senior devs it's not quite as intense, because while we would be annoyed at not having the useful tool to help things along we still retain the knowledge and expertise to do the research, design, coding and reasoning ourselves. However, don't be fooled, it is making you dependent and atrophying your mind as well. Albeit maybe at a slower pace, because it's like you're working with a junior programmer you're having to correct.

Anyway, that's my two cents on the matter.

Disclaimer: Sorry for being so verbose, suspicious I know. But, this comment is entirely written by a real human, in an age of mankind where that is now hard to prove.

Don't take me as a luddite, I think the LLM are amazing and I love seeing it grow and develop and all the cool new things being created with and for it. The ecosystem is really getting impressive now.

Edit: I'd like to add an additional danger is that we'll no longer train junior devs, we'll just train AI. The opportunities will no longer be there for them. And, as a result, the junior devs will never grow up to be senior devs. They'll never learn from a mentor and improve.