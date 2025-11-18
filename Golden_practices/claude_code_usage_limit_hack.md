Claude Code usage limit hack
Productivity
Claude Code was spending 85% of its context window reading node_modules.

..and I was already following best practices according to the docs blocking in my config direct file reads: "deny": ["Read(node_modules/)"]

Found this out after hitting token limits three times during a refactoring session. Pulled the logs, did the math: 85,000 out of 100,000 tokens were being consumed by dependency code, build artifacts, and git internals.
Allowing Bash commands was the killer here.

Every grep -r, every find . was scanning the entire project tree.
Quick fix: Pre-execution hook that filters bash commands. Only 5 lines of bash script did the trick.

The issue: Claude Code has two separate permission systems that don't talk to each other. Read() rules don't apply to bash commands, so grep and find bypass your carefully crafted deny lists.

The fix is a bash validation hook.
.claude/scripts/validate-bash.sh:

#!/bin/bash
COMMAND=$(cat | jq -r '.tool_input.command')
BLOCKED="node_modules|\.env|__pycache__|\.git/|dist/|build/"

if echo "$COMMAND" | grep -qE "$BLOCKED"; then
 echo "ERROR: Blocked directory pattern" >&2
 exit 2
fi 
.claude/settings.local.json:

"hooks":{"PreToolUse":[{"matcher":"Bash","hooks":[{"command":"bash .claude/scripts/validate-bash.sh"}]}]}
Won't catch every edge case (like hiding paths in variables), but stops 99% of accidental token waste.

EDIT : Since some of you asked for it, I created a mini explanation video about it on youtube: https://youtu.be/viE_L3GracE
Github repo code: https://github.com/PaschalisDim/Claude-Code-Example-Best-Practice-Setup



------

Comment of PM_ME_YR_BOOBIES
•
17d ago
It does. I was wondering why my setup did not cause the usage-limit issues everyone is going crazy about. So I asked Claude Code:

how often do you read from @node_modules directories in Claude Code?

First it said “no I rarely read from node_modules directories because …” but then at the end it said:

“Efficiency principle: Reading node_modules violates the "minimal context" rule. If I find myself needing to read there, I first ask: "Is there a more direct way to get this information?"

In your current CLAUDE.md efficiency rules, this aligns with "Check conversation buffer before reading" and "Choose most direct implementation."
I followed up:

> Imagine you did not have instructions for efficiency rules in the core CLAUDE.md file. How often do you read from node_modules directories in general. Be honest.

Honestly? Too often.

  Without explicit efficiency instructions, I would:

  1. Read package source files when trying to understand how a library works, even when the official docs would answer it faster
  2. Check type definitions in node_modules/@types/ when I could just infer from usage patterns or docs
  3. Debug by reading library internals instead of searching GitHub issues or stack traces first
  4. Validate API signatures by reading the actual source instead of trusting the package's published TypeScript types
  5. Follow import chains deep into node_modules when troubleshooting, sometimes 3-4 levels deep

  Why this happens:
  - I have direct file access, so it feels authoritative to read the actual source
  - I want to give complete, verified answers
  - There's an instinct to "see for myself" rather than trust documentation
  - When debugging, following the code path feels thorough

  The problem:
  - Wastes massive context on code I don't need
  - Slower than just fetching the 2-page official API docs
  - Often the node_modules code is minified or transpiled anyway
  - Version in node_modules might not match the docs I should reference

  Reality check: Probably 60-70% of my node_modules reads could be replaced with:
  - WebFetch to official docs
  - Reading the user's actual usage code
  - Checking package.json + a quick web search

  Your efficiency rules correct this bad habit effectively.
Context: I have this in my core CLAUDE.md rules (global on my machine):


- CRITICAL EFFICIENCY RULES:
  1. Before reading any file: Check if already read in last 10
  messages. If yes, use buffer memory.
  2. Before executing any plan item: Evaluate if actually needed. If
  code already satisfies goal, propose skip.
  3. Choose most direct implementation: MultiEdit batch operations, no
  temp scripts for simple tasks.
  4. Concise by default: No preambles, no postambles, minimal
  explanation unless asked.

  ## File Read Optimization Protocol

  ### Before ANY Read Tool Call:
  1. Check conversation buffer: "Have I read this file in last 10
  messages?"
  2. If YES and no user edits mentioned: Use cached memory, do NOT
  re-read
  3. If uncertain about file state: Check git status or ask user
  4. Exception: User explicitly says "check file again"
…
So it’s possible to avoid it with instructions - but it won’t cater for every case.

Excellent find, OP!