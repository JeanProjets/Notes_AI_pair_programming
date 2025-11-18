I built a context management plugin and it CHANGED MY LIFE
Showcase
Okay so I know this sounds clickbait-y but genuinely: if you've ever spent 20 minutes re-explaining your project architecture to Claude because you started a new chat, this might actually save your sanity.

The actual problem I was trying to solve:

Claude Code is incredible for building stuff, but it has the memory of a goldfish. Every new session I'd be like "okay so remember we're using Express for the API and SQLite for storage and—" and Claude's like "I have never seen this codebase in my life."

What I built:

A plugin that automatically captures everything Claude does during your coding sessions, compresses it with AI (using Claude itself lol), and injects relevant context back into future sessions.

So instead of explaining your project every time, you just... start coding. Claude already knows what happened yesterday.

How it actually works:

Hooks into Claude's tool system and watches everything (file reads, edits, bash commands, etc.)

Background worker processes observations into compressed summaries

When you start a new session, last 10 summaries get auto-injected

Built-in search tools let Claude query its own memory ("what did we decide about auth?")

Runs locally on SQLite + PM2, your code never leaves your machine

Real talk:

I made this because I was building a different project and kept hitting the context limit, then having to restart and re-teach Claude the entire architecture. It was driving me insane. Now Claude just... remembers. It's wild.

Link: https://github.com/thedotmack/claude-mem (AGPL-3.0 licensed)

It is set up to use Claude Code's new plugin system, type the following to install, then restart Claude Code.

/plugin marketplace add thedotmack/claude-mem

/plugin install claude-mem
Would love feedback from anyone actually building real projects with Claude Code, if this helps you continue, if it helps you save tokens and get more use out of Claude Code. Thanks in advance!