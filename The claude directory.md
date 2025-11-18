The `.claude/` directory is the key to supercharging dev workflows! 🦾
r/ClaudeCode - The `.claude/` directory is the key to supercharging dev workflows! 🦾
I've been rockin' with a very basic `.claude/` directory that simply contains a simple `settings.json` file for months. This approach has worked well but I definitely felt like there was room for improvement.

Recently, I spun up some subagents, commands, and hooks in a side project I've been working on. The attached image shows my updated `.claude/` directory. I am loving this new approach to AI-assisted development!

🤖 Subagents act as experts focused on specific areas. For example, I have an "MCP Transport Expert" and a "Vector Search Expert". These subagents can work on very specific tasks in parallel.

⌨️ Commands allow you to define custom slash commands. Are you frequently prompting Claude Code to "Verify specs have been fully implemented..."? Just create a "/verify-specs" command!

🪝 Hooks allow you to introduce some determinism to inherently probabilistic workflows. For example, you can ensure that linting, typechecking, and tests run after each subagent completes its task.

I highly recommend investing time into optimizing use of the `.claude/` directory! 🦾


Repo with full .claude directory now available here: https://github.com/Matt-Dionis/claude-code-configs

Example of the directory : 

.claude/
├─ agents/
│  ├─ code-reviewer.md
│  ├─ companion-architecture.md
│  ├─ debugger.md
│  ├─ mcp-protocol-expert.md
│  ├─ mcp-sdk-builder.md
│  ├─ mcp-transport-expert.md
│  ├─ mcp-types-expert.md
│  ├─ memory-architecture.md
│  ├─ memory-lifecycle.md
│  ├─ memory-validator.md
│  ├─ neon-drizzle-expert.md
│  ├─ pgvector-advanced.md
│  ├─ production-deployment.md
│  ├─ test-runner.md
│  └─ vector-search-expert.md
├─ commands/
│  ├─ explain.md
│  ├─ mcp-debug.md
│  ├─ memory-ops.md
│  ├─ perf-monitor.md
│  ├─ review.md
│  ├─ setup.md
│  └─ test.md
├─ hooks/
│  ├─ lint-check.sh
│  └─ typescript-dev.sh
├─ settings.json
├─ settings.local.example.json
└─ settings.local.json
