The CEO of Decide just shared a brilliant way to use AI for coding I came across a post from the CEO of Decide — an AI data analyst tool — and his workflow for coding with AI is honestly genius.

Instead of jumping straight into “write this code,” here’s what he does:

Uploads all the relevant project files so the AI gets full context. Lets the LLM read and understand the codebase first. Describes what changes or features he wants (without asking for code yet). Asks the AI to propose three different implementation strategies and critique each one. Then selects the best approach and moves forward with coding.

This approach basically turns the AI into a thoughtful collaborator rather than just a code generator. It helps the model reason about the problem before it writes a single line.


stingraycharles
•
11d ago
Profile Badge for the Achievement Top 1% Commenter Top 1% Commenter
Yeah, as a matter of fact, I ask it to identify ambiguities, contradictions or gaps in the specification and ask me 10 clarifying questions. I then go back to the original prompt and add that information in there. Rinse & repeat until I got a very detailed spec that leaves very little room open for interpretations.

Works very well and ensures it doesn’t make stupid choices.


turboronin
•
11d ago
I found out that when asking for a list of somethings (i.e.: clarifying questions, options, etc.) it works best if you don't specify a set number. These models have compliance built in their training: if you ask for 10 options, the model will be compelled to give you 10. If there were only, say. 5 real options then it's going to make up the other 5. My 2c.

Cryptoslazy
•
11d ago
one more thing, if you are working with libraries.. make sure to input the library docs in plain text format. so it will use latest documentation example to write code.

otherwise it will keep writing obsolete code.

i think from now on, each library should come with LLM friendly docs. so for example when working with cursor or qoder. it would check the documentation (with best practices) so i think it will write cleaner code.


heavy-minium
•
11d ago

There is a simpler rule being all of this: let the LLM do thing in smaller steps so that it doesn't skip any important step.

So don't tell it to fix the bug directly - first it has to identify the bug and verify that it could really be related to the observation of the bug.

You also don't let it implement the prompted task directly, but first it must outline the requirements and add any missing non-functional requirements that may be necessary. In short, go through the steps a (good) engineer should go through.

The part about coming up with three different ways to implement and then critique each of them - that might work, but that's like 4-5x more token consumptions and you're waiting a looonngg time, just for a modest increase in accuracy. Very wasteful, it's going to do that even for the simplest tasks that have only one good solution.

dCrumpets
•
10d ago
The solution is to use one session for brainstorming, dump a summary into a doc, use a new session to create a detailed implementation plan, dump it into a doc or jira via acli, then use a session per ticket / PR. Using git worktrees is very helpful once you get to implementation.