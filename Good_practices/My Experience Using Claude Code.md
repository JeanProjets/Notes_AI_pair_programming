My Experience Using Claude Code
Let me walk you through my research workflow and how I use it.

So let’s say that I want to learn a new topic in RL. And I found this repo to fit my needs. I would simply clone this repo and open Claude Code in this directory.


Once opened, I can ask anything about the repo. For example `> can you give the structure of the reinforcement learning master folder?`


Then I can ask it to `Explore the DP directory? I want to know and study what’s in that directory.`


I can even ask it to `Implement Dynamic Programming algorithms in a new notebook? Ensure that it is simple and easy to understand.`

In such a case, Claude Code will create a to-do list of things that it wants to achieve.


You can see the entire process, and if it’s ready, it will prompt you to decide whether to take the next step.


Once it successfully ticks off all the Todos, it creates the Notebook for you.

In my experience, it is very reflective. I know what is happening and if I want to take a step further. As a researcher, it perfectly fits into my workflow.

I can even monitor my API usage.

I can spin up different Claude Code and work in parallel in the same codebase. It is fast. It helps me to explore ideas and find solutions that might fit my needs.

It lets you be a part of the process by constantly inviting you to add your ideas or stop or resume the process.

It is intuitive as well.

Share

Key Features
Claude Code boosts my output tenfold. It turns research into learning and exploration. That’s why its agentic nature matters most in fast-paced workflow.

I saw this firsthand when I was trying to implement Reinforcement Learning with Verifiable Rewards or RLVR in my codebase. The tool scanned my codebase alone. It planned steps. Then executed edits across files. Unlike GitHub Copilot's inline hints, Claude acts independently. No constant tweaks needed.

All it asked was if I needed to implement the changes in the existing codebase (like the one shown before).

Also, if I need to, I can interrupt the scanning or thinking process and later ask it to continue from where I left off. Because it writes the code in a temporary state and doesn’t disturb/change the existing codebase.

Agentic coding sits at the core of Claude Code. It reads queries/prompts. Gathers context via Bash. Makes changes. Some of the supporting features build on Claude Code,

Universal fit works in any terminal or IDE like VS Code.

GitHub integration lets me `@Claude` on issues for auto-fixes.

Advanced models like Opus 4 one-shot tests perfectly.

Then we have Claude.md files. It groups instructions logically.


Source: Manage Claude's memory
Root (/Library/Application/Support/ClaudeCode/Claude.md) for repo rules.

Global (~/.claude/Claude.md) for all projects.

Use # to add memory details on the fly.


Why terminal?

Technically, the terminal choice enables rapid iteration. A simple interface means quick updates. Anthropic engineers iterated fast on diverse stacks. That's a monopoly edge over bloated alternatives.

Try testing it on a small task. Maybe just download an existing repo and prompt it to “Explain the repo”, “create a comprehensive outline of the repo and stack used”, etc.


Share Adaline Labs

Installation and Setup
Proper setup ensures Claude Code integrates reliably. That’s why precise installation matters for consistent results.

I observed that Node.js is the core requirement. Many developers have it. Without it, setup fails outright.

Prerequisites focus on the basics.

Node.js (version 18 or higher recommended, tested on 20.x).

Supported OS: macOS 10.15+, Ubuntu 20.04+, Debian 10+, or Windows 10+ with WSL or Git Bash.

At least 4GB RAM for smooth operation.

Anthropic API key from their dashboard.

Step-by-step keeps it straightforward.

Open your terminal.

Install globally: npm install -g @anthropic-ai/claude-code

Navigate to your project directory where you want to run Claude Code.

Run: claude to start Claude Code.

Go through the basic setup steps and you are ready to go.

Common pitfalls include outdated Node.js, causing install errors. Update with nvm or official installers. Check compatibility in diverse terminals like iTerm2 or VS Code.

Verification confirms success.

Type `claude` and prompt: "Describe your version." It responds with details if set up correctly.

Refer to official docs for updates.

Usage and Examples
Claude Code sparks ideas into reality. It accelerates product cycles like nothing else. That's why its usage drives true leverage in development.

I found it shines in quick tweaks. Say you want to improve a feature’s logic. Prompt it simply. It rethinks the core, suggests refinements. Suddenly, your prototype evolves without hours lost.

For deeper flows, I used it to streamline team reviews. Describe a product gap in an issue. It outlines fixes, drafts updates. This cut our iteration time in half during a recent sprint.

Think about your next prototype. How much faster could you validate assumptions? Envision turning vague concepts into testable builds overnight.

Pro tip: Layer in ongoing notes via memory. Tell it to recall past decisions. It keeps your vision consistent across sessions.

With tools like Claude Code, you can create faster prototypes, which means quicker market tests. Start prompting today. Reflect on one product idea. See where it leads.

Evaluate Prompt

Limitations and Potential Drawbacks
Claude Code has several limitations as well.

Cost hits hardest for heavy users. I burned through $150 in a month on prototypes. It adds up when scaling products.

This matters in solo ventures or startups. Budgets tighten. You hesitate on deep dives.

Switch to Claude Max. It caps worries with unlimited access. I did, and usage soared without fear.

Complex tasks falter next. The tool stumbles on intricate data models. It can't always be one-shot interactions between components.

I saw this in a multi-system refactor. Claude got stuck midway. Code needed tweaks.

Problems arise in large codebases or novel architectures. Prompts alone fall short.

Start with plans. Ask for outlines first. I add context by letting it read files upfront. Then intervene manually for the final polish.

Older models demanded repeats to steer right. Newer ones like Opus 4 fix that mostly.

Yet intuitions reset with each upgrade. I retest assumptions per release.

Track changes in docs. Adjust prompts accordingly. Stay ahead.

My Learnings
I share my experience from a research perspective, but it does fit into almost any workflow. I am eager to share more thoroughly from the product perspective as well, where you can provide a screenshot to Claude Code, and it will write the code.

Yes, Claude Code is a multimodal terminal.

It is extremely versatile, fun, and responsive. But only if you write the prompt with great detail. After all, it is built on top of language models. So prompt engineering is the key.

Instructions that you often repeat can be stored in the memory folder. This aligns the model with your thinking, purpose, and goals. And also, some things I felt were prone to context overload, especially when it tries to refactor the entire notebook.

So, working with a proper plan, phases, and chunks is what I recommend.

Overall, Claude Code is a good tool for researchers, developers, and even product leaders. I will make more content on Claude Code from a product perspective and share how such agentic tools can increase productivity.