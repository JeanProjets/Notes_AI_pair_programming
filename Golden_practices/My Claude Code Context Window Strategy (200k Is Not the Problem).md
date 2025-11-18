I Finally Cracked My Claude Code Context Window Strategy (200k Is Not the Problem)

I’ve been meaning to share this for a while: here’s my personal Claude Code context window strategy that completely changed how I code with LLMs.

If you’ve ever thought “200k tokens isn’t enough” – this post is for you. Spoiler: the problem usually isn’t the window size, it’s how we burn tokens.

1 – Context Token Diet: Turn OFF Auto-Compact Most people keep all the “convenience” features on… and then wonder where their context went.

The biggest hidden culprit for me was Auto Compact.

With Auto Compact ON, my session looked like this:

85k / 200k tokens (43%)

After I disabled it in /config:

38k / 200k tokens (19%)

That’s more than half the initial context usage gone, just by turning off a convenience feature.

My personal rule:

🔴 The initial context usage should never exceed 20% of the total context window.

If your model starts the session already half-full with “helpful” summaries and system stuff, of course it’ll run out of room fast.

“But I Need Auto Compact To Keep Going…?”

Here’s how I work without it.

When tokens run out, most people: 1. Hit /compact 2. Let Claude summarize the whole messy conversation 3. Continue on top of that lossy, distorted summary

The problem: If the model misunderstands your intent during that summary, your next session is built on contaminated context. Results start drifting. Code quality degrades. You feel like the model is “getting dumber over time”.

So I do this instead: 1. Use /export to copy the entire conversation to clipboard 2. Use /clear to start a fresh session 3. Paste the full history in 4. Tell Claude something like: “Continue from here and keep working on the same task.”

This way: • No opaque auto-compacting in the background • No weird, over-aggressive summarization ruining your intent • You keep rich context, but with a clean, fresh session state

Remember: the 200k “used tokens” you see isn’t the same as the raw text tokens of your conversation. In practice, the conversation content is often ~100k tokens or less, so you do still have room to work.

Agentic coding is about productivity and quality. Auto Compact often kills both.

2 – Kill Contaminated Context: One Mission = One Session The second rule I follow:

🟢 One mission, one 200k session. Don’t mix missions.

If the model goes off the rails because of a bad prompt, I don’t “fight” it with more prompts.

Instead, I use a little trick: • When I see clearly wrong output, I hit ESC + ESC • That jumps me back to the previous prompt • I fix the instruction • Regenerate

Result: the bad generations disappear, and I stay within a clean, focused conversation without polluted context hanging around.

Clean session → clean reasoning → clean code. In that environment, Claude + Alfred can feel almost “telepathic” with your intent.

3 – MCP Token Discipline: On-Demand Only Now let’s talk MCP.

Take a look at what happens when you just casually load up a bunch of MCP tools: • Before MCPs: 38k / 200k tokens (19%) • After adding commonly used MCPs: 133k / 200k tokens (66%)

That’s two-thirds of your entire context gone before you even start doing real work.

My approach: • Install MCPs you genuinely need • Keep them OFF by default • When needed: 1. Type @ 2. Choose the MCP from the list 3. Turn it ON, use it 4. Turn it OFF again when done

Don’t let “cool tools” silently eat 100k+ tokens of your context just by existing.

“But What About 1M Token Models Like Gemini?”

I’ve tried those too.

Last month I burned through 1M tokens in a single day using Claude Code API. I’ve also tested Codex, Gemini, Claude with huge contexts.

My conclusion:

🧵 As context gets massive, the “needle in a haystack” problem gets worse. Recall gets noisy, accuracy drops, and the model struggles to pick the right pieces from the pile.

So my personal view:

✅ 200k is actually a sweet spot for practical coding sessions if you manage it properly.

If the underlying “needle in a haystack” issue isn’t solved, throwing more tokens at it just makes a bigger haystack.

So instead of waiting for some future magical 10M-token model, I’d rather: • Upgrade my usage patterns • Optimize how I structure sessions • Treat context as a scarce resource, not an infinite dump

My Setup: Agentic Coding with MoAI-ADK + Claude Code

If you want to turn this into a lifestyle instead of a one-off trick, I recommend trying MoAI-ADK with Claude Code for agentic coding workflows.

👉 GitHub: https://github.com/modu-ai/moai-adk

If you haven’t tried it yet, give it a spin. You’ll feel the difference in how Claude Code behaves once your context is: • Lean (no unnecessary auto compact) • Clean (no contaminated summaries) • Controlled (MCPs only when needed) • Focused (one mission per session)

If this was helpful at all, I’d really appreciate an upvote or a share so more people stop wasting their context windows. 🙏