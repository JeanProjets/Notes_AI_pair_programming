# 📖 The Bible of AI Pair Programming
## A Comprehensive Guide to Mastering AI-Driven Development with Claude Code

---

## Table of Contents
1. [SETUP: Prepare Your Workspace](#setup)
2. [PLANNING: Think Before You Code](#planning)
3. [IMPLEMENTATION: Build with Precision](#implementation)
4. [REVIEW: Quality Assurance & Iteration](#review)
5. [Advanced Patterns & Optimization](#advanced)
6. [Common Pitfalls & How to Avoid Them](#pitfalls)

---

# SETUP: Prepare Your Workspace {#setup}

> **The Golden Rule of Setup**: Your environment determines your output. Invest time here to save 10x time later.

## Phase 1: Context Window Optimization

### Understanding Your Context Budget

The 200k token context window is NOT about size—it's about **how you spend it**.

**The Reality:**
- Initial context bloat kills productivity
- A poorly optimized session uses 85k+ tokens before you even start working
- 200k is a sweet spot IF managed properly

**Your Target:**
- 🔴 **Initial context should never exceed 20% of total window** (40k out of 200k)
- Anything beyond that is wasted space that could be productive work

### Step 1: Disable Auto-Compact

**Why:** Auto-Compact kills code quality through lossy summarization

**How to disable:**
1. Access Claude Code settings
2. Turn OFF "Auto-Compact" in preferences
3. Verify you're at ~19% context usage at session start

**When context runs out:** Instead of hitting `/compact`:
1. Use `/export` to copy entire conversation
2. Use `/clear` to start fresh session
3. Paste full history at top
4. Tell Claude: "Continue from here on the same task"
5. Result: Clean context, no contamination, preserved intent

### Step 2: Manage MCP Servers Ruthlessly

**The Problem:**
```
Before MCPs: 38k / 200k tokens (19%)
After casual MCP loading: 133k / 200k tokens (66%)
```
That's 2/3 of your context consumed before coding starts.

**The Solution:**
- Install only MCPs you genuinely need
- Keep them OFF by default
- Activate on-demand:
  1. Type `@`
  2. Select MCP from list
  3. Turn ON when needed
  4. Turn OFF immediately after use

**Your MCP Discipline:**
```
DO: Install essential MCP → Use @mention → Turn off
DON'T: Load bunch of MCPs "just in case"
```

### Step 3: Set Initial Context Limit

In your Claude Code settings, configure:
```
Maximum initial context usage: 20% (40k tokens)
Reserve for work: 80% (160k tokens)
```

## Phase 2: CLAUDE.md Setup (Core Memory System)

`CLAUDE.md` is Claude's persistent memory. It automatically loads into EVERY session in that directory.

### Where to Place CLAUDE.md Files

```
repo-root/
├── CLAUDE.md                 ← Main instructions (shared with team)
├── CLAUDE.local.md           ← Personal notes (.gitignored)
└── subdirectories/
    └── CLAUDE.md             ← Subproject-specific rules
```

### What Goes in Root CLAUDE.md (Keep Under 200 lines)

**1. Quick Start Section**
```markdown
# Quick Start
- pnpm install
- pnpm dev
- npm run build
- npm run test
```

**2. Critical Universal Rules**
```markdown
# MUST ALWAYS
- Run `npm run typecheck` after code changes
- Check BEST_PRACTICES.md for patterns
- Never commit without running tests
- Use exact file paths when testing
```

**3. Project Structure**
```markdown
# Key Files
- src/app.tsx - Entry point
- src/api/routes.ts - API endpoints
- src/db/schema.ts - Database schema
```

**4. Workflow Instructions**
```markdown
# Development Workflow
1. Create feature branch: git checkout -b feature/name
2. Write tests FIRST
3. Implement code
4. Run: npm run build && npm run test
5. Commit with semantic commit message
```

**5. Code Style & Preferences**
```markdown
# Code Style
- Use TypeScript strict mode
- Import destructuring: import { foo } from 'bar'
- 2-space indentation
- Comment public functions
```

### Tuning Your CLAUDE.md

**Common Mistake:** Overloading with too much detail

**Solution:** Iterate like a prompt
- Start with essentials
- Observe what Claude ignores
- Test emphasis techniques: "IMPORTANT:", "YOU MUST:"
- Keep content under 200 lines

**Anti-patterns:**
```
❌ 30-page essay about your codebase
❌ Every single rule ever conceived
❌ Outdated information

✅ High-signal, actionable instructions
✅ Quick-reference format
✅ Team-agreed conventions
```

## Phase 3: Skills System (Reusable Knowledge)

Skills are portable, reusable guidelines that Claude can reference. Unlike CLAUDE.md (project-specific), skills are (domain-specific).

### When to Create a Skill

Create a skill when you have:
- Reusable patterns across projects
- Best practices for a domain (frontend, backend, databases)
- Complex decision trees you don't want to repeat
- Utility scripts and helpers

### Skill Structure (Best Practices)

**Main Skill File: KEEP UNDER 500 LINES**
```
skill-name/
├── SKILL.md                    ← Main file (300-400 lines)
└── resources/
    ├── patterns.md             ← Detailed patterns
    ├── examples.md             ← Code examples
    ├── faq.md                  ← Common questions
    └── test-auth.js            ← Utility script
```

**Why Progressive Disclosure?**
- Claude loads lightweight main file initially
- Only pulls detailed resources when needed
- Token efficiency: 40-60% reduction

### Example Skill Structure

```markdown
# Backend Development Guidelines

## Quick Reference
- Routes → Controllers → Services → Repositories
- Error handling with Sentry
- Prisma for database access

## Key Patterns
1. Controller pattern
2. Service layer pattern
3. Repository pattern

## Attached Resources
- See `resources/patterns.md` for detailed patterns
- See `test-auth.js` for testing authenticated endpoints
```

### Auto-Activation with Hooks (Game Changer)

The problem: Claude won't use skills unless explicitly told.

The solution: **Force activation with hooks**

```typescript
// .claude/hooks/user-prompt-submit.ts

// Analyzes your prompt BEFORE Claude sees it
// Injects skill activation reminder

function analyzePromptForSkills(userPrompt: string) {
  const keywords = {
    'backend': 'backend-dev-guidelines',
    'frontend': 'frontend-dev-guidelines',
    'database': 'database-verification',
    'auth': 'backend-dev-guidelines',
  }

  for (const [keyword, skill] of Object.entries(keywords)) {
    if (userPrompt.toLowerCase().includes(keyword)) {
      return `🎯 SKILL ACTIVATION: Use ${skill} skill`
    }
  }
}
```

**Result:** When you ask about backend code, Claude SEES the skill reminder BEFORE reading your question. Skills actually get used.

## Phase 4: Hooks System (Automation Guard Rails)

Hooks run automatically in response to events. They're your QA pipeline.

### Essential Hooks to Implement

**Hook 1: Build Checker (Post-Response)**
```typescript
// Runs AFTER Claude finishes responding
// Catches TypeScript errors immediately

1. Reads which files were modified
2. Runs build script on affected repos
3. If < 5 errors: Shows them to Claude
4. If ≥ 5 errors: Recommends error-resolver agent
5. Claude fixes before moving on

Result: Zero errors left behind
```

**Hook 2: Error Handling Reminder (Post-Response)**
```
Analyzes edited files after Claude finishes
Detects risky patterns (try-catch, async, database)
Shows gentle reminder: "Did you add error handling?"

Non-blocking, just awareness
```

**Hook 3: Skill Auto-Activation (Pre-Prompt)**
```
Analyzes your prompt for keywords
Checks which skills are relevant
Injects reminder: "Use X skill for this task"

Claude loads skill before even reading your prompt
```

**Example Hook Configuration:**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": ".*",
        "hooks": [
          {"command": "bash .claude/scripts/activate-skills.sh"}
        ]
      }
    ],
    "StopEvent": [
      {
        "hooks": [
          {"command": "bash .claude/scripts/check-builds.sh"}
        ]
      }
    ]
  }
}
```

### Protecting Against Context Waste

**The Hidden Problem:** Bash commands bypass Read() rules

If you have `"deny": ["Read(node_modules/)"]`, Claude can still:
```bash
grep -r "pattern" .          # Scans node_modules anyway
find . -name "*.json"        # Wastes 85% of context
```

**The Fix: Bash Validation Hook**
```bash
#!/bin/bash
# .claude/scripts/validate-bash.sh

COMMAND=$(cat | jq -r '.tool_input.command')
BLOCKED="node_modules|\.env|\.git/|dist/|__pycache__"

if echo "$COMMAND" | grep -qE "$BLOCKED"; then
  echo "ERROR: Blocked directory pattern" >&2
  exit 2
fi
```

---

# PLANNING: Think Before You Code {#planning}

> **The Fundamental Truth**: Plans save 10x the time they take to create.

## Phase 1: One Mission = One Session

### The Principle

Never mix multiple missions in a single session. One goal, one 200k window.

**Why?**
- Contaminated context from unrelated work
- Claude loses focus and drifts
- Code quality degrades
- Harder to debug

### When Context Gets Poisoned

If Claude generates something clearly wrong:
1. Press **ESC + ESC** immediately
2. Jump back to previous prompt
3. Fix the instruction
4. Regenerate
5. Bad generations disappear, context stays clean

## Phase 2: Strategic Planning Workflow

### The Three-Phase Process

#### Phase 1: Exploration
Let Claude explore the codebase with **grep** or **glob**

**Starter prompts:**
```markdown
"Generate complete tree-like hierarchy of entire repository,
showing all directories and .tsx files in clear, indented format"

"Trace dependency flow starting from app.tsx. Show hierarchy of
imported modules. Present as clear tree structure illustrating
how codebase connects together"
```

**Goal:** Build understanding of codebase structure

#### Phase 2: Planning (WITH THINKING)
Instruct Claude to brainstorm WITHOUT coding yet.

**Key phrase:** Use "think" or "think harder" to activate extended thinking:
```markdown
"THINK: Plan a feature to add rate limiting. Include:
- Which endpoints need protection
- Storage mechanism for rate data
- Error responses and status codes
- Integration points with existing middleware

Then critique your own plan. What did you miss?"
```

**Extended Thinking Levels:**
- `think` = moderate computation time
- `think hard` = more thinking budget
- `think harder` = extensive analysis
- `ultrathink` = maximum reasoning

**Output:** Comprehensive written plan with:
- Executive summary
- Phases/steps
- Tasks checklist
- Risks and mitigations
- Success metrics
- Timelines

#### Phase 3: Approval & Documentation
- Review plan thoroughly (this catches 80% of mistakes)
- Ask clarifying questions
- Have Claude critique its own plan
- Save plan as `instructions.md`
- Reference it in every subsequent prompt

### The File Mapping Strategy (Game Changer)

Before coding ANY feature, create a CSV roadmap:

```csv
Status,File,Priority,Lines,Complexity,Depends On,What It Does,Progress Notes
TODO,types.ts,CRITICAL,200,Medium,Database,All TypeScript interfaces,
TODO,api.service.ts,CRITICAL,300,High,types.ts,API client class,
TODO,useHook.ts,HIGH,400,High,api.service.ts,Main state hook,
DONE,Component.tsx,HIGH,250,Medium,hooks,UI component,Added auto-save
```

**Why this works:**
1. Claude builds 3 files at a time, then STOPS
2. Updates CSV with actual progress
3. Never loses track of what's done
4. Can see divergence between plan and reality
5. Dependencies are crystal clear

**Process:**
```
1. Sit with schema/requirements
2. Build CSV with Claude
3. "Build next 3 TODO items"
4. "Test them, update CSV to DONE"
5. Repeat until everything DONE
```

## Phase 3: Dev Docs System (Prevents Context Loss)

The problem: Claude is "extremely confident junior dev with extreme amnesia." Gets lost easily.

Solution: Dev Docs—3 files that persist the plan during implementation.

### The Three Sacred Documents

**1. [task-name]-plan.md** - The approved plan
```markdown
# Feature: User Authentication

## Overview
Implement JWT-based authentication system

## Phases
1. Create User model
2. Create auth routes
3. Create middleware
4. Integration tests

## Timeline
- Phase 1: 2 hours
- Phase 2: 3 hours
- Phase 3: 2 hours
```

**2. [task-name]-context.md** - Key decisions & relevant files
```markdown
# Task Context

## Key Files
- src/auth/controller.ts (line 45) - existing auth logic
- src/db/models/User.ts - user schema
- src/api/routes.ts - where to add routes

## Important Decisions
- Using JWT (not sessions)
- Refresh tokens stored in DB
- 30-minute expiration

## Gotchas
- Database migration needed for refresh_tokens table
- Existing auth middleware needs updating
```

**3. [task-name]-tasks.md** - Checklist with status
```markdown
# Implementation Checklist

## Phase 1: Create User Model
- [ ] Create User entity
- [ ] Add timestamps
- [ ] Add validation

## Phase 2: Create Auth Routes
- [ ] POST /auth/register
- [ ] POST /auth/login
- [ ] POST /auth/refresh

## Phase 3: Middleware
- [ ] Implement JWT verification
- [ ] Test authenticated endpoints

Last Updated: 2025-11-18 14:30
```

### How to Use Dev Docs

**Before starting large task:**
```bash
mkdir -p ~/git/project/dev/active/[task-name]/
# Create three files above
```

**When exiting plan mode:**
1. Claude generates plan
2. You review it thoroughly
3. Claude creates three dev docs files
4. You mark as "APPROVED"
5. Claude implements feature while updating docs

**During implementation:**
1. Claude works in focused chunks
2. Updates progress notes regularly
3. When context runs low, run `/update-dev-docs`
4. Claude syncs all changes
5. Start fresh session with `/clear` and "continue"

**The Magic:** Claude ALWAYS knows where it is, never loses track of original intent.

## Phase 4: Design-to-Code Pipelines

For UI-heavy features, use visual iteration:

### Process

1. **Get Visual Reference:**
   - Screenshot from Figma
   - Copy screenshot to clipboard
   - Paste directly into Claude Code (`Ctrl+V`)

2. **Ask Claude to Implement:**
   ```markdown
   "Implement this design in React. Focus on:
   - Layout and spacing (use CSS Grid/Flexbox)
   - Color scheme (extract from design)
   - Responsive behavior (mobile/tablet/desktop)
   - Typography hierarchy"
   ```

3. **Iterate 2-3 Times:**
   - Claude takes screenshot of result
   - You compare against original
   - Claude adjusts styling
   - Usually looks great after 2-3 iterations

### Best for Prototypes

⚠️ Note: Works best for new features, not ideal for:
- Updating existing components
- Handling multiple Figma frames
- Complex interactions

---

# IMPLEMENTATION: Build with Precision {#implementation}

> **The Core Philosophy**: Structure your work so mistakes are caught before you see them.

## Phase 1: Starting Implementation

### Before Claude Writes Any Code

**1. Have Claude Create a Specific Implementation Plan**
```markdown
"Before implementing, create a step-by-step plan:
1. Which files need to be created/modified?
2. What's the dependency order?
3. Where will you start?
4. How will you test each part?"

Don't write code yet. Just plan.
```

**2. Review the Plan**
- Does it match your approval?
- Any obvious gaps?
- Have Claude critique it

**3. Only Then: "Okay, implement step 1"**

### Chunked Implementation Pattern

Don't ask Claude to build everything at once.

```markdown
WRONG:
"Build the entire checkout flow"

RIGHT:
"Build ONLY the CheckoutForm component next.
Don't touch anything else.
Include inline validation.
When done, we'll test, then move to next file."
```

**Why:**
- Easier to review
- Catch errors quickly
- Don't compound mistakes
- Opportunity to course-correct

## Phase 2: Writing and Testing

### The Edit-Test Loop (TDD with AI)

**Step 1: Claude writes failing test**
```markdown
"Write a test for the validateEmail function that:
- Accepts valid emails (user@domain.com)
- Rejects invalid emails (no @, multiple @, empty)
- Handles edge cases (unicode, very long domains)
- Returns Result<(), ValidationError>"
```

**Step 2: You review the test**
- Does it test the right behavior?
- Is it realistic?
- Missing any cases?

**Step 3: "Now make this test pass. Don't modify tests."**

**Step 4: Claude iterates**
- Writes code
- Runs tests
- Fixes failures
- Repeats until passing

**Result:** Code that's proven to work, not guessed

### Code Style Guidelines

Put this in CLAUDE.md or a skill:

```markdown
# Code Style Rules

## TypeScript
- Use strict mode
- Explicit error handling (no unwraps)
- Document public functions
- Keep functions under 50 lines

## React
- Use hooks, not class components
- Memoize expensive computations
- Lift state when needed
- Use composition over inheritance

## Error Handling
- Never silent failures
- Log with context
- Sentry for production errors
- User-facing error messages

## Database (Prisma)
- Always use repository pattern
- Handle transaction errors
- No N+1 queries
- Migration before schema changes
```

## Phase 3: Autonomous Workflows with Checkpoints

### Letting Claude Work Between Checkpoints

The key: Claude works autonomously, but YOU stay active.

```markdown
"Execute the following plan:
1. Create User model with timestamps
2. Create auth service with login/register
3. Create auth middleware
4. Create tests for auth flow

Work through step by step.
After each step, show me what you created.
Don't move to next step until I approve."
```

**When to interrupt (hit ESC):**
- Claude is going off-track
- Taking too long on one task
- You realize mid-execution: "wait, wrong approach"

**When to continue:**
- Everything looks good
- Progress is smooth
- Code quality is high

### The Review Loop

After Claude creates code:

**Automatic Code Review Checkpoint:**
```markdown
"Now review what you just created:
1. Does it match the requirements?
2. Any edge cases missed?
3. Error handling complete?
4. Performance issues?
5. Security concerns?"
```

**You then review:**
- Security: injection, auth, secrets
- Performance: N+1 queries, loops, allocations
- Correctness: edge cases, error paths
- Style: follows guidelines

## Phase 4: Scripts & Utility Helpers

When Claude helps you write a useful script, **document it immediately**.

**Example: Auth Testing Script**

```javascript
// scripts/test-auth-route.js
// Usage: node scripts/test-auth-route.js http://localhost:3000/api/users

const https = require('https');

async function testAuthRoute(url) {
  // 1. Get refresh token from auth service
  // 2. Sign token with JWT secret
  // 3. Create cookie header
  // 4. Make authenticated request
  // 5. Log response
}
```

**Document in CLAUDE.md or attach to skill:**
```markdown
### Testing Authenticated Routes

Use the provided test-auth-route.js script:

```bash
node scripts/test-auth-route.js http://localhost:3000/api/endpoint
```

This handles:
- JWT token creation
- Cookie signing
- HTTP request
- Response validation
```

## Phase 5: Handling Multiple Repositories

### The PM2 Process Manager Pattern

If working with multiple backend services:

```javascript
// ecosystem.config.js
module.exports = {
  apps: [
    {
      name: 'auth-service',
      script: 'npm',
      args: 'start',
      cwd: './services/auth',
      out_file: './services/auth/logs/out.log',
      error_file: './services/auth/logs/error.log',
    },
    {
      name: 'api-gateway',
      script: 'npm',
      args: 'start',
      cwd: './services/gateway',
    },
    // ... more services
  ]
};
```

**Command:**
```bash
pnpm pm2:start  # Starts all services with logging
pm2 logs auth --lines 200  # View service logs
pm2 restart auth  # Restart service
```

**Why this matters:**
- Claude can read live logs: `pm2 logs email --lines 200`
- Spot errors immediately
- Services auto-restart on crash
- Real-time debugging without manual log fetching

## Phase 6: Context Management During Implementation

### When to Use /clear

Clear context when:
- Finished one feature, starting a different one
- Conversation has gotten messy
- Too much old history distracting from current task
- Context usage creeping above 70%

**Don't clear if:**
- Still working on same feature
- Might need to reference earlier decisions
- Mid-iteration on same code

### The Token Awareness Rule

Monitor your context budget:
- **50% full:** Still good, keep going
- **70% full:** Start thinking about /clear or /compact soon
- **85% full:** Plan your next action carefully
- **95% full:** Time to /clear or /export → /clear → paste back

### Update Dev Docs Before /clear

Before clearing context:
```markdown
"/update-dev-docs

Update the three task files with:
1. Any new context learned
2. Current progress status
3. Next steps needed
4. Any blockers to resolve"
```

This ensures continuity when you clear.

---

# REVIEW: Quality Assurance & Iteration {#review}

> **The Principle**: You can't trust output you haven't reviewed. Treat Claude like a junior developer.

## Phase 1: Security Review

**For every PR/changeset, ask:**

1. **Injection Vulnerabilities**
   - SQL injection: Check database queries
   - XSS: Check any DOM manipulation
   - Command injection: Check shell executions
   - Template injection: Check string interpolation

2. **Authentication & Authorization**
   - Is auth checked before sensitive operations?
   - Are tokens validated?
   - Are permissions enforced?
   - Any hardcoded secrets?

3. **Data Protection**
   - Sensitive data encrypted?
   - Logs don't expose secrets
   - No credentials in comments
   - .gitignore updated

**Prompt Claude:**
```markdown
"Security review of changes to src/auth/:
1. Check for any injection vectors
2. Verify JWT validation is correct
3. Look for hardcoded secrets
4. Check error messages don't leak info"
```

## Phase 2: Performance Review

**Questions to ask:**

1. **Database Queries**
   - Any N+1 query patterns?
   - Are indexes being used?
   - Unnecessary joins?
   - Batch operations where possible?

2. **Algorithmic Efficiency**
   - Nested loops? O(n²)?
   - Unnecessary computations?
   - Can you cache results?
   - Premature optimization vs actual bottleneck?

3. **Memory & Resources**
   - Large data structures in loops?
   - Memory leaks?
   - Unnecessary allocations?
   - Proper cleanup?

**Prompt Claude:**
```markdown
"Performance review of changes to src/api/quotes:
1. Are there any N+1 query patterns?
2. Could we batch these database calls?
3. Any caching opportunities?
4. Algorithm complexity OK?"
```

## Phase 3: Correctness Review

1. **Edge Cases**
   - Empty input?
   - Null/undefined handling?
   - Boundary conditions?
   - Maximum values?

2. **Error Paths**
   - What happens on failure?
   - Are all exceptions caught?
   - User-facing errors clear?
   - Logging for debugging?

3. **Integration**
   - Does it integrate with existing systems?
   - Are breaking changes documented?
   - Database migrations needed?
   - API contract changes?

**Prompt Claude:**
```markdown
"Correctness review:
1. What happens if email service is down?
2. How do we handle partial failures?
3. Are all database transactions atomic?
4. What about rate limiting?"
```

## Phase 4: Style & Consistency

**Create a code review checklist:**

```markdown
# Code Review Checklist

## Style
- [ ] Follows naming conventions
- [ ] Consistent formatting
- [ ] Comments where needed
- [ ] No dead code
- [ ] No console.log() left behind

## Tests
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Edge cases covered
- [ ] Error cases tested

## Documentation
- [ ] Public functions documented
- [ ] Complex logic explained
- [ ] API changes documented
- [ ] README updated if needed

## Performance
- [ ] No obvious N+1 queries
- [ ] Reasonable algorithm complexity
- [ ] No memory leaks
- [ ] Proper resource cleanup
```

**Prompt Claude to review against checklist:**
```markdown
"Review your changes against this checklist: [paste checklist]
Mark each item ✅ or ❌ with explanation"
```

## Phase 5: Testing Your Code

### Three Levels of Testing

**1. Unit Tests**
```markdown
"Write unit tests for validateEmail function:
- Valid emails pass
- Invalid emails fail
- Edge cases (unicode, long domains)
- Return correct error types"
```

**2. Integration Tests**
```markdown
"Write integration test for checkout flow:
1. Create user
2. Add item to cart
3. Process payment
4. Verify order in database
5. Verify email sent"
```

**3. Manual Testing (When Critical)**
```markdown
"Test the authentication flow manually:
1. Register new user
2. Login with email
3. Refresh token
4. Logout
5. Try to access protected endpoint (should fail)"
```

### Diagnostic Reports

When stuck, ask for systematic breakdown:

```markdown
"Generate diagnostic report:
1. List all files modified in last session
2. Explain role of each file
3. Why is current error occurring?
4. Propose 3 different debugging approaches
5. Which approach would you recommend?"
```

This forces Claude to think systematically instead of guess-and-check.

## Phase 6: Git & Commit Workflow

### Commit Granularity

```bash
# Use interactive staging
git add -p

# This lets you:
# 1. Review each change
# 2. Commit logically related changes together
# 3. Keep diffs readable
# 4. Easy to revert specific changes if needed
```

### Commit Message Standards

```
feat: Add JWT authentication system

- Create User model with hashed passwords
- Implement login/register endpoints
- Add auth middleware
- Include tests for all auth flows

Closes #123
```

**Format:**
```
<type>: <subject>

<body explaining why, not what>
<link to issue if applicable>
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `refactor:` Code reorganization
- `test:` Test additions
- `perf:` Performance improvement

### Pushing to Branch

```bash
# First push uses -u to set upstream
git push -u origin feature/auth

# Subsequent pushes
git push
```

---

# ADVANCED PATTERNS & OPTIMIZATION {#advanced}

## Pattern 1: Multi-Claude Verification

Run two Claude instances in parallel:

1. **Claude A:** Writes code
2. **Claude B (separate context):** Reviews code

**Setup:**
```bash
# Terminal 1
claude  # Start Claude A for implementation

# Terminal 2
claude  # Start Claude B for review

# Claude B can see Claude A's work in shared folder
```

**Process:**
1. Claude A writes feature
2. Claude B reviews for bugs, security, style
3. Claude A reads review and fixes issues
4. Separate context = no bias, fresh perspective

## Pattern 2: Git Worktrees for Parallel Work

For independent tasks, use git worktrees:

```bash
# Create worktree for feature A
git worktree add ../project-auth feature/auth
cd ../project-auth
claude  # Start Claude A here

# Create worktree for feature B (from main terminal)
git worktree add ../project-dashboard feature/dashboard
cd ../project-dashboard
claude  # Start Claude B here

# Each Claude works independently
# No merge conflicts, shared git history
```

## Pattern 3: Custom Slash Commands

Create reusable workflows with slash commands.

**File:** `.claude/commands/code-review.md`
```markdown
# Code Review Command

## Purpose
Comprehensive code review of your changes

## Instructions
1. Read diff of recent changes
2. Review for:
   - Security vulnerabilities
   - Performance issues
   - Code style consistency
   - Missing error handling
   - Test coverage
3. List issues with severity
4. Provide specific fixes
```

**Usage:**
```bash
/code-review  # Expands to full review prompt
```

## Pattern 4: Context7 MCP for Documentation Sync

Keep documentation current without copying outdated snippets:

```bash
# Install Context7 MCP
# Configure in .mcp.json
# Claude always fetches latest docs

Instead of:
"Here's our auth docs..." [paste docs from 2 weeks ago]

Use:
"@Context7: Fetch latest auth documentation"
```

## Pattern 5: Codebase Summaries

For large projects, use gitingest.com:

```bash
# Go to gitingest.com
# Enter your GitHub URL (or modify URL):
# github.com/user/repo → gitingest.com/user/repo
# Download generated summary
# Reference in prompts instead of pasting files
```

**Instead of:**
```
[Pasting 10 files into prompt]
```

**Do this:**
```markdown
"Based on the codebase summary attached (codebase_summary.txt):
[your specific question]"
```

---

# COMMON PITFALLS & HOW TO AVOID THEM {#pitfalls}

## Pitfall 1: "200k Isn't Enough"

**What you're probably doing:**
- Auto-Compact turned ON
- 5+ MCPs loaded all the time
- Dumping entire project into context
- Reading node_modules

**What to do instead:**
- Turn OFF Auto-Compact
- Keep MCPs OFF by default
- Use gitingest for summaries
- Block node_modules reads
- Context bloat is a workflow issue, not a window size issue

## Pitfall 2: Starting to Code Without a Plan

**Wrong approach:**
```
"Build me a checkout flow"
→ Claude starts coding
→ 2 hours in: "Wait, what are we building?"
→ Wasted time, inconsistent code
```

**Right approach:**
```
1. "Think: Plan a checkout flow"
2. Review plan with Claude
3. Create file mapping CSV
4. Create dev docs
5. THEN start coding

Saves 10x the planning time
```

## Pitfall 3: Trusting AI Without Review

**The mistake:**
- Claude writes code
- You assume it's correct
- Go to production
- Security issue or bug found

**The truth:**
- Claude is extremely smart at pattern matching
- Claude is NOT wise about context and implications
- Treat every change like junior developer PR
- Review security, performance, correctness

**What to do:**
- Always security review
- Always test
- Always review edge cases
- Use automated checks (linters, type checking)

## Pitfall 4: Mixing Multiple Missions

**Problem:**
- Work on feature A for 2 hours
- Then ask: "While we're here, can you fix feature B?"
- Claude gets confused
- Both features are half-done
- Context contaminated

**Solution:**
- One mission per 200k session
- When finished, `/clear` and start fresh
- Or use git worktrees for parallel work

## Pitfall 5: Dumping Context, Expecting Magic

**Wrong:**
```
"Here's my entire codebase (50 files).
Why doesn't authentication work?"
```

**Right:**
```
"@src/auth.ts line 85 panics on None when JWT is malformed.
Fix this and add proper error handling."
```

Specific problems get specific solutions.

## Pitfall 6: Not Documenting as You Go

**Problem:**
- Build features quickly
- Forget WHY you built them certain ways
- Claude forgets context
- Next developer can't understand

**Solution:**
- Dev docs after every feature
- Comments for complex logic
- Architecture diagrams
- Decision logs

## Pitfall 7: Skills Ignored by Claude

**Problem:**
- Create comprehensive skills
- Tell Claude about them once
- Claude never references them again

**Solution:**
- Use hooks to force activation
- Explicitly reference in prompts: "Use the backend-dev-guidelines skill"
- Keep skills under 500 lines (progressive disclosure)
- Test what actually gets used

## Pitfall 8: Context Loss After /compact

**Problem:**
- Hit /compact mid-feature
- Claude forgets intent
- Code quality degraded
- Feel like Claude "got dumb"

**Solution:**
- Use dev docs instead of /compact
- Before /compact, save all context to files
- /clear + paste back is better than /compact
- Fresh session state without lossy summarization

## Pitfall 9: Asking for Mind-Reading

**Wrong:**
```
"Make it production-ready"
"Optimize it"
"Make it better"
```

**Right:**
```
"Make it production-ready by:
1. Adding comprehensive error handling
2. Adding Sentry error tracking
3. Writing unit tests for all functions
4. Documenting all public APIs"
```

## Pitfall 10: Not Using Extended Thinking

**Problem:**
- Complex decisions need more analysis
- Claude makes quick assumptions
- Wrong architectural choices

**Solution:**
```markdown
"THINK HARD: Design the database schema for a multi-tenant SaaS

Consider:
- Data isolation
- Performance at scale
- Query patterns
- Backup/restore
- Migration strategy"
```

Extended thinking activates deeper reasoning.

---

# SUMMARY: Your Daily Workflow

## Morning: Setup (5 minutes)

```bash
# 1. Start fresh Claude Code session
claude

# 2. Verify context at startup
# Should see: "X% / 200k tokens" (aim for <20%)

# 3. Your CLAUDE.md automatically loads
# Check that it's present and accurate
```

## Mid-Morning: Planning (15-30 minutes)

```markdown
"THINK HARD: [describe feature or fix]

Create a detailed plan with:
- Overview of what we're building
- Files that need creation/modification
- Phases with dependency order
- Testing strategy
- Timeline estimate"
```

## Late Morning: File Mapping (10 minutes)

```markdown
"Create a CSV with all files needed for this feature:

Columns: Status, File, Priority, Lines, Complexity,
Dependencies, Description, Progress Notes

Make it comprehensive but realistic."
```

## Mid-Day: Dev Docs Creation (15 minutes)

```markdown
"Create three markdown files:
1. [feature]-plan.md - our approved plan
2. [feature]-context.md - key decisions and file references
3. [feature]-tasks.md - checklist with phases"
```

## Afternoon: Implementation (2-4 hours)

```markdown
"Implement next 3 TODO items from CSV:
- Follow all guidelines in backend-dev-guidelines skill
- Write tests as you go
- Update CSV when complete
- Run builds before considering done"
```

## Late Afternoon: Review (30-60 minutes)

```markdown
"Review what we just built:
1. Security check
2. Performance check
3. Test coverage
4. Error handling
5. Code style"
```

## End of Day: Commit & Document (10 minutes)

```bash
git add -p
git commit -m "feat: [feature name]

[description of changes]"

git push -u origin feature/[name]
```

---

# Your Personal Mantra

Print this and put it on your monitor:

```
🎯 PLAN FIRST
   30 minutes planning saves 6 hours refactoring

📚 DOCUMENT AS YOU GO
   Future you will thank present you

🔍 REVIEW EVERYTHING
   Catch bugs before they catch you

🎯 ONE MISSION, ONE SESSION
   Contaminated context kills productivity

🚀 ITERATE, DON'T ITERATE
   2-3 passes over code catches 80% of issues

⚙️ AUTOMATE YOUR CHECKS
   Hooks, tests, and scripts catch errors for you

💾 VERSION CONTROL IS YOUR SAFETY NET
   Small commits = easy rollback

🧠 TREAT CLAUDE LIKE JUNIOR DEV
   Smart at patterns, not wisdom about context
```

---

# Reference: Key Commands Cheat Sheet

```bash
# Context Management
/clear              # Start fresh session (clears history)
/compact            # Summarize session (lossy - avoid)
/export             # Copy conversation to clipboard
/init               # Generate starter CLAUDE.md

# File Operations
@file/path.ts       # Reference specific file
@file/path.ts:42-88 # Reference specific lines

# Tools & Extensions
@toolname           # Enable/disable MCP tools
/permissions        # Manage allowed tools

# Planning Mode
THINK               # Activate thinking (basic)
THINK HARD          # More computation
THINK HARDER        # Extensive analysis
ULTRATHINK          # Maximum reasoning

# Interrupts
ESC                 # Interrupt Claude (preserves context)
ESC ESC             # Jump to previous prompt (edit & retry)

# Hooks
.claude/hooks/      # Custom hook directory
.mcp.json          # MCP configuration
```

---

# Final Truth

The engineers seeing massive gains with Claude Code aren't using magic prompts.

They're using **disciplined workflows**.

They:
- 📋 Plan before coding
- 🔐 Review before pushing
- 📚 Document everything
- 🔄 Iterate deliberately
- ⚙️ Automate their checks

This Bible is your map.

Follow it religiously, and Claude Code becomes not just a tool—it becomes an extension of your thinking.

**The future of coding isn't human vs. AI.**

**It's humans with AI vs. humans without it.**

Choose your side wisely.

---

*Last Updated: 2025-11-18*
*This Bible synthesizes 6+ months of hardcore AI pair programming best practices*
