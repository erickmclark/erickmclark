# Master Claude Code - Complete How-To Guide
## From Beginner to Expert (Based on Boris Cherny's Techniques)

**Goal:** Learn every technique Boris uses to build 10x faster with Claude Code.

---

## 🎯 Quick Start: The 3 Most Important Things

Before diving deep, these 3 habits compound into 10x productivity:

### 1. Always Give Claude a Way to Verify (MOST IMPORTANT)
**Why:** Claude iterates until the result is perfect when it can test.

```
❌ WRONG:
"Build a landing page"
(Claude builds it, you hope it looks good)

✅ RIGHT:
"Build a landing page. I'll open it in a browser and test it. 
Fix anything that looks wrong until I say it's perfect."
(Claude can iterate, you verify, it gets great)
```

**The Magic:** Browser = feedback loop = 3x quality improvement.

### 2. Use Plan Mode Before Building
**Why:** A good plan = 1-shot implementation. A bad plan = infinite iterations.

```
Shift+Tab (twice) to enter Plan mode
→ Describe what you want
→ Claude proposes a plan
→ You iterate on the plan (not the code!)
→ Once plan is solid, Claude builds it right the first time
```

### 3. Keep a CLAUDE.md File
**Why:** Claude learns from past mistakes and builds better next time.

```
# CLAUDE.md (in your repo)

## Patterns That Work
- Use `bun` not `npm`
- Prefer TypeScript interfaces over types
- Test with `bun test`

## Gotchas (Avoid These)
- ❌ Never use enums (use string literals instead)
- ❌ Avoid async in useEffect without cleanup
- ❌ Don't hardcode API keys

## Architecture Notes
- Database: Supabase (auth + realtime)
- Frontend: Next.js 15 Turbopack
- Testing: Vitest (faster than Jest)
```

Claude reads this and doesn't repeat mistakes.

---

# 📚 Complete How-To Library

## SECTION 1: PARALLEL EXECUTION
### Run 5+ Claude Sessions at Once

**Why:** Instead of waiting for one Claude to finish, run 5 in parallel. 5x speed.

### How-To: Terminal Parallel Sessions

**Step 1: Open 5 terminal tabs**
```bash
# Tab 1: cd ~/my-repo && claude
# Tab 2: cd ~/my-repo && claude
# Tab 3: cd ~/my-repo && claude
# Tab 4: cd ~/my-repo && claude
# Tab 5: cd ~/my-repo && claude
```

**Step 2: Use git worktrees (no conflicts)**
```bash
# Instead of 5 sessions in same folder, use worktrees
git worktree add .claude/wt1 origin/main
git worktree add .claude/wt2 origin/main
git worktree add .claude/wt3 origin/main

# Now each Claude can work independently:
# Tab 1: cd .claude/wt1 && claude "Build auth"
# Tab 2: cd .claude/wt2 && claude "Build dashboard"
# Tab 3: cd .claude/wt3 && claude "Build API"

# Each works in isolation, no git conflicts
# When done, merge: git merge .claude/wt1, etc.
```

**Step 3: Monitor all 5 at once**
```bash
# In a 6th tab, watch all:
watch 'ps aux | grep claude | wc -l'  # Shows how many running

# Or see which ones are done:
ls -lh .claude/wt*  # See modified times
```

**Step 4: Collect results**
```bash
# Each Claude makes commits in its worktree
# Merge them back (order matters - start with foundational work):
git merge .claude/wt1  # auth foundation first
git merge .claude/wt2  # dashboard (depends on auth)
git merge .claude/wt3  # API (depends on both)
```

**Real Example:** Building NightPivot
```
Terminal 1: claude "Build database schema and migrations"
Terminal 2: claude "Build API endpoints (auth, CRUD)"
Terminal 3: claude "Build landing page (hero, pricing, CTA)"
Terminal 4: claude "Build admin dashboard (stats, logs)"
Terminal 5: claude "Build client SDK for integrations"

All 5 run in parallel. If one takes 2 hours, others keep working.
Instead of 10 hours serial → 2-3 hours parallel.
```

**Pro Tip: Name Your Tabs**
```bash
# In iTerm2: Cmd+Shift+L to set label
# Label them: [Auth] [API] [Frontend] [Admin] [SDK]
# Makes it easy to see what's running where
```

---

## SECTION 2: PLAN MODE MASTERY
### Build Perfect Plans So Claude 1-Shots Implementation

**The Problem:**
- You ask Claude to build something
- Claude starts coding
- You realize halfway through it's wrong direction
- Have to redo everything
- Huge waste of time

**The Solution: Plan Mode**
- Enter plan mode BEFORE building
- Iterate on the PLAN (cheap, fast)
- Once plan is solid, Claude builds it right the first time

### How-To: Use Plan Mode

**Step 1: Start a session**
```bash
claude "Build a user authentication system"
```

**Step 2: Enter Plan Mode**
```
Shift+Tab (once) = Set expectations
Shift+Tab (twice) = Enter full plan mode

You should see: "▮▮ plan mode on"
```

**Step 3: Claude proposes a plan**
```
Claude shows something like:

▮▮ PLAN: User Authentication System
1. Database schema (users table, auth columns)
2. API endpoints: /auth/signup, /auth/login, /auth/logout
3. Password hashing (bcrypt)
4. JWT tokens for session
5. Middleware for protected routes
6. Testing with sample requests

Is this the right direction?
```

**Step 4: Iterate on the plan (not code)**
```
You: "I want OAuth support too (Google, GitHub). 
And I need role-based permissions (admin, user, guest).
Add those to the plan."

Claude updates the plan:
1. Database schema (users + oauth_providers + roles tables)
2. OAuth integration (Google, GitHub)
3. Role-based access control (RBAC)
4. API endpoints (signup, login, oauth callback, etc.)
... etc

Perfect? → Say "Go ahead, build it"
```

**Step 5: Claude builds the full implementation**
```
Once you approve the plan, Claude implements the whole thing
in one shot, often without needing corrections.

This is the magic: good plan → 1-shot build → no rework
```

**Real Example: NightPivot Landing Page**

```
Initial ask: "Build a landing page"

Plan Mode shows:
1. Hero section (headline, subheading, CTA)
2. How It Works (3 steps with icons)
3. Services (6 automation categories)
4. Pricing (3 tiers: audit, build, managed)
5. FAQ section
6. Footer

You: "I want to showcase customer results too. 
Add a testimonials section.
And social proof (# of customers, ROI stats)."

Claude updates plan:
1. Hero
2. How It Works
3. Services
4. Customer Results & Testimonials
5. Social Proof Stats
6. Pricing
7. FAQ
8. Footer

"Perfect. Build it."

→ Claude builds entire landing page in one go
→ No back-and-forth, no rework
```

### When to Use Plan Mode
- ✅ Any task >30 minutes
- ✅ When you're unsure of the direction
- ✅ When there are multiple approaches
- ✅ Before major refactors
- ✅ Before building with multiple components

### When to Skip Plan Mode
- ❌ Quick fixes (<5 min)
- ❌ Simple tasks ("add a button", "fix typo")
- ❌ Tasks you've done 100 times before

---

## SECTION 3: CLAUDE.MD - TEACH CLAUDE NOT TO REPEAT MISTAKES

**The Concept:** CLAUDE.md is like a personal style guide for Claude. It learns from your past mistakes.

### How-To: Create and Use CLAUDE.md

**Step 1: Create .claude/CLAUDE.md in your repo**
```bash
mkdir -p .claude
cat > .claude/CLAUDE.md << 'EOF'
# CLAUDE.md - NightPivot Style Guide

## Tech Stack Decisions
- Frontend: Next.js 15 (not 14), use Turbopack
- Database: Supabase (auth built-in)
- API: Express.js (lightweight, fast routing)
- Testing: Vitest (not Jest, 3x faster)
- Package manager: bun (not npm, faster)

## Code Style
- Use TypeScript `interface` over `type`
- Prefer functional components over class
- Use hooks, not HOCs
- Always add error handling for API calls

## Gotchas (Don't Do These)
- ❌ Never use enums (use string literal unions instead)
- ❌ Don't hardcode API keys in code
- ❌ Avoid deep nesting in components (extract to subcomponents)
- ❌ Don't forget .env.local in .gitignore
- ❌ Async/await in useEffect without cleanup

## Database Patterns
- Always use RLS (Row Level Security) for Supabase
- Create migrations for every schema change
- Use foreign keys with ON DELETE CASCADE carefully
- Index columns you query frequently

## API Patterns
- All endpoints should validate input with zod
- Return consistent error format: { success, error, data }
- Log all errors to Sentry/monitoring
- Add rate limiting (1000 req/hr per IP)

## Performance
- Lighthouse score must be >90
- Load time target: <2 seconds
- Bundle size: <200KB gzipped
- Cache API responses >1 hour if possible

## Testing
- Test happy path + 3 error cases minimum
- Integration tests for API endpoints
- E2E tests for critical user flows
- Run: bun run test:all before committing

## Recent Learnings
- [Date] Discovered enum bug: use string literal unions instead
- [Date] API timeout was 5s, increased to 30s
- [Date] Supabase realtime expensive, use polling for non-critical
EOF
```

**Step 2: Add to git and share with Claude**
```bash
git add .claude/CLAUDE.md
git commit -m "Add CLAUDE.md style guide"

# Claude automatically reads .claude/CLAUDE.md at start of each session
```

**Step 3: When Claude makes a mistake, add it to CLAUDE.md**

```
Claude writes: if (user.role == 'admin') { ... }

You: "Use enums for role checks, not string comparisons."

Claude: "Good catch. Let me fix that and add to CLAUDE.md"

Claude updates CLAUDE.md:
- ❌ Never use string comparison for roles: if (user.role == 'admin')
- ✅ Use enum/union type: if (user.role === ROLES.ADMIN)

Next time, Claude remembers this pattern.
```

**What to Put in CLAUDE.md**

| Category | Examples |
|----------|----------|
| Tech Decisions | Framework versions, languages, databases |
| Code Style | Prefer interfaces, no enums, functional components |
| Gotchas | "Don't use X", "Always do Y" |
| Performance | Lighthouse targets, bundle size limits |
| Testing | Test coverage minimums, which flows to test |
| Architecture | Database patterns, API design, file structure |
| Recent Learnings | Bugs found, optimizations made, decisions made |

**Real Example: Boris's CLAUDE.md (from his tweets)**
```
## Always Use
- bun run typecheck (before commits)
- bun run test:all (required before PR)
- Chrome extension for web work verification

## Never Use
- npm (use bun)
- enums (use string literal unions)
- async in useEffect without cleanup

## Common Mistakes I Made
- Tried Firebase realtime, was expensive → use Supabase
- Used Redux, too complex → use Zustand
- Hardcoded paths, breaks in production → use env vars

## What I Learned Last Month
- Worktrees are the productivity unlock (can run 5 Claudes in parallel)
- Verification is everything (give Claude a browser to test)
- Plan mode saves 60% of dev time (plan solid, build once)
```

---

## SECTION 4: GIT WORKTREES FOR PARALLEL WORK

**The Problem:** If you run Claude in same folder, they conflict (both editing same files).

**The Solution:** Git worktrees = isolated copies of same repo.

### How-To: Set Up Worktrees

**Step 1: Create worktrees**
```bash
git worktree add .claude/wt1 origin/main
git worktree add .claude/wt2 origin/main
git worktree add .claude/wt3 origin/main
```

Each is a separate working directory, same repo.

**Step 2: Start Claude in each**
```bash
# Terminal 1
cd .claude/wt1
claude "Build authentication system"

# Terminal 2
cd .claude/wt2
claude "Build dashboard with charts"

# Terminal 3
cd .claude/wt3
claude "Build API endpoints"
```

**Step 3: Work in parallel**
All 3 Claudes work at the same time, no conflicts.

**Step 4: Merge results back**
```bash
# When all are done, merge into main
git merge .claude/wt1
git merge .claude/wt2
git merge .claude/wt3

# Clean up worktrees
git worktree remove .claude/wt1
git worktree remove .claude/wt2
git worktree remove .claude/wt3
```

**Merge Order Matters**
- Start with foundational work (database, auth)
- Then middleware and utilities
- Then features that depend on above
- Finally UI/frontend

**Real Example: Building NightPivot**
```
Main branch: main
├── .claude/wt1: Build database + migrations
│   └── Commit: "Add Supabase schema for leads, clients, workflows"
├── .claude/wt2: Build API (depends on db)
│   └── Commit: "Add /api/clients, /api/leads, /api/audit endpoints"
├── .claude/wt3: Build landing page (no dependencies)
│   └── Commit: "Add hero, services, pricing, CTA sections"
└── .claude/wt4: Build admin dashboard
    └── Commit: "Add stats page, client list, workflow monitor"

Merge order: wt1 → wt2 → wt3 → wt4 (foundation first, UI last)
```

---

## SECTION 5: MOBILE APP & REMOTE CONTROL

**The Idea:** You can code from your phone. Claude responds.

### How-To: Use Claude on Your Phone

**Step 1: Download Claude iOS/Android App**
- Go to App Store or Google Play
- Search "Claude"
- Download Anthropic's official Claude app

**Step 2: Open Code Tab**
- Launch app
- Tap "Code" at bottom
- You're in coding mode

**Step 3: Ask Claude to Build Something**
```
"Fix the login button styling. 
Make it red and add a loading spinner."
```

Claude reviews your code, suggests fixes, you approve.

**Step 4: See Changes Live**
Claude shows diffs. You can approve/reject.

**Real Use Case:**
- You're on a call with a client
- They say "Can you make the button bigger?"
- You pull out your phone, ask Claude in the app
- Claude shows the change
- You approve on your phone
- Change is pushed to production

### How-To: Remote Control (Dispatch)

**The Idea:** Your laptop runs Claude. You control it from anywhere.

**Step 1: Enable Remote Control in Settings**
```
In Claude Code app or CLI:
/config
→ Enable Remote Control for all sessions
```

**Step 2: Start a Session**
```bash
# On your laptop
claude "Build a complex feature"
```

**Step 3: Control from Your Phone**
- Phone: Claude app → Code tab
- Type: `/remote-control`
- Claude on laptop responds to your commands from your phone

**Step 4: Or Use Dispatch (Web)**
- Visit: https://dispatch.anthropic.com
- Login with your Claude account
- See all your laptop's sessions
- Control from anywhere (web browser)

**Real Example:**
- You're at a coffee shop
- Need to fix production bug on your laptop at office
- Open Dispatch in mobile browser
- Control your Claude session remotely
- Bug fixed in 5 minutes, zero setup time

---

## SECTION 6: HOOKS FOR AUTOMATION

**The Idea:** Run code at specific points in Claude's lifecycle.

### How-To: Set Up Hooks

**Step 1: Create .claude/hooks.json**
```json
{
  "SessionStart": [
    {
      "type": "command",
      "command": "echo 'Session started. Loading context...'"
    },
    {
      "type": "load_context",
      "files": ["CLAUDE.md", ".env.local"]
    }
  ],
  "PreToolUse": [
    {
      "matcher": "Write|Edit",
      "hooks": [
        {
          "type": "validate",
          "script": "bun run lint:file -- $FILE"
        }
      ]
    }
  ],
  "PostToolUse": [
    {
      "matcher": "Write|Edit",
      "hooks": [
        {
          "type": "command",
          "command": "bun run format || true"
        }
      ]
    }
  ],
  "PermissionRequest": [
    {
      "matcher": ".*",
      "hooks": [
        {
          "type": "route_to_slack",
          "channel": "approvals",
          "message": "Claude wants permission: $REQUEST"
        }
      ]
    }
  ]
}
```

**Step 2: What Each Hook Does**

| Hook | When | What It Does |
|------|------|------------|
| SessionStart | Claude starts | Load context, run setup |
| PreToolUse | Before Claude uses a tool | Validate inputs |
| PostToolUse | After Claude uses a tool | Auto-format code, run tests |
| PermissionRequest | Before Claude runs something risky | Route to Slack for approval |
| Stop | When Claude gets stuck | Automatically unstick it |

**Real Example: Auto-Formatting**
```json
{
  "PostToolUse": [
    {
      "matcher": "Write",
      "hooks": [
        {
          "type": "command",
          "command": "bun run format:write -- $FILE && bun run lint:file -- $FILE"
        }
      ]
    }
  ]
}
```

Every time Claude writes a file:
1. Auto-format it
2. Auto-lint it
3. If lint fails, Claude sees errors and fixes them

Result: Perfect code without you asking.

---

## SECTION 7: VOICE DICTATION (GAME CHANGER)

**The Reality:** You speak 3x faster than you type.

### How-To: Use Voice with Claude Code

**Option 1: CLI Voice**
```bash
claude
# Type: /voice
# Hold SPACEBAR to talk
# Release to send
```

**Option 2: Desktop App Voice**
- Click microphone icon
- Talk to Claude
- It listens and responds

**Option 3: iOS Voice**
- Enable dictation in Settings
- Talk to Claude in the app

**Why It Works:**
- Speaking = you describe in detail
- Detailed prompts = better code
- 3x faster than typing
- You don't think about grammar, just ideas

**Real Example:**
```
❌ Typed: "Add auth"

✅ Spoken: "Add email and password authentication. 
Users click sign up, enter email and password, 
we hash the password with bcrypt, store in Supabase, 
return a JWT token that's stored in local storage. 
Add protected routes that check the token. 
If expired, redirect to login."

More detail = better code the first time.
```

---

## SECTION 8: VERIFICATION IS EVERYTHING

**Boris's #1 Rule:** If Claude can't verify its work, quality drops 3x.

### How-To: Set Up Verification

**For Web Apps:**
```bash
# Install Chrome extension (or use Desktop app)
# Claude can now open browser, see rendered page
# It tests, sees issues, fixes them automatically

Example:
Claude: "I'll build the landing page and test it in the browser"
→ Claude writes code
→ Claude opens browser
→ Claude sees hero section is misaligned
→ Claude fixes CSS
→ Claude verifies it looks good
→ Done (no human testing needed)
```

**For APIs:**
```bash
# Set up test endpoint
POST /api/test-request

Claude builds API:
→ Writes endpoint code
→ Tests with curl/Postman
→ Checks response format
→ Fixes any errors
→ Verifies with multiple test cases
```

**For Databases:**
```bash
# Give Claude access to query database
SELECT * FROM users WHERE ...

Claude builds database feature:
→ Writes schema migration
→ Tests query
→ Verifies data integrity
→ Adds indexes if needed
```

**Real Example: Building NightPivot**

Without verification:
```
Claude: "I built the landing page"
You: Open localhost:3002
You: "Hero text is cut off on mobile. 
     Pricing button styling is wrong. 
     Footer is overlapping."
You: Ask Claude to fix
Claude: Fixes
You: Test again
Claude: Fixes more
... (5-6 iterations)
```

With verification:
```
Claude: "I'll build the landing page and test it in a browser"
→ Claude builds
→ Claude opens browser at localhost:3002
→ Claude tests on desktop, tablet, mobile
→ Claude sees issues and fixes them
→ Claude verifies mobile looks good
→ Done in 1-2 iterations (10x faster)
```

---

## SECTION 9: /BATCH - PARALLELIZE SUBAGENTS

**The Idea:** When a task is too big for one Claude, spawn 10 (or 100).

### How-To: Use /batch

**Step 1: Ask Claude to use batch**
```
"Migrate all sync functions to async.
Batch up the work and launch 10 parallel agents to handle it.
Each agent handles one file, tests it, opens a PR."
```

**Step 2: Claude creates a batch job**
Claude automatically:
- Breaks work into 10 chunks
- Spawns 10 subagents
- Each works independently
- Each creates a PR
- You review & merge PRs

**Real Example: Code Migration**
```
Task: "Convert 50 components from class to functional"

Without /batch:
- Claude: 8 hours (one at a time)

With /batch:
- /batch "Convert to functional, 10 agents in parallel"
- 10 agents each convert 5 components
- 10 PRs appear within 30 minutes
- You review & merge
- Done in <1 hour (8x faster)
```

### Real Example: Database Migration
```
Task: "Migrate all tables from MySQL to Postgres, 
update queries in 200 files, test everything"

Without /batch:
- Claude: 20 hours
- Tons of manual testing

With /batch:
- /batch "Parallel migration: 20 agents
  Each takes 10 files, runs tests, creates PR"
- 20 PRs appear in 1-2 hours
- Each has tests passing
- Merge them one by one
- Total: 2-3 hours (10x faster)
```

---

## SECTION 10: CUSTOM AGENTS

**The Idea:** Create specialized agents for specific tasks.

### How-To: Create Custom Agents

**Step 1: Create .claude/agents/ directory**
```bash
mkdir -p .claude/agents
```

**Step 2: Create agent definition**
```bash
cat > .claude/agents/CodeReviewer.md << 'EOF'
---
name: CodeReviewer
description: Reviews code for bugs, security, performance
color: blue
tools: Read, Comment
model: opus  # Use best model for code review
---

You are an expert code reviewer.
Your job: Find bugs, security issues, performance problems.
Style: Direct, actionable feedback.
Always suggest fixes, not just complaints.

# Checklist
- [ ] Security: No SQL injection, auth issues, secrets exposed
- [ ] Performance: No N+1 queries, unnecessary loops
- [ ] Tests: Adequate coverage, edge cases tested
- [ ] Readability: Clear variable names, easy to follow logic
- [ ] Patterns: Follows codebase conventions
EOF
```

**Step 3: Use your custom agent**
```bash
claude --agent=CodeReviewer
# Now you have a specialized reviewer agent
```

**Real Example: Create Multiple Agents**
```
.claude/agents/
├── CodeReviewer.md       # Reviews code
├── ArchitectureAdvisor.md # Advises on design
├── SecurityAuditor.md     # Checks for security issues
├── PerformanceOptimizer.md # Optimizes speed
└── DocumentationWriter.md # Writes docs
```

**Use Them in Parallel:**
```bash
# Tab 1: claude --agent=CodeReviewer "Review all controllers"
# Tab 2: claude --agent=SecurityAuditor "Check auth system"
# Tab 3: claude --agent=PerformanceOptimizer "Profile and optimize"

# 3 specialized agents work in parallel
# Each is expert in their domain
```

---

## SECTION 11: SLASH COMMANDS

**The Idea:** Shortcuts for common tasks. Type `/command` instead of full prompts.

### How-To: Create Slash Commands

**Step 1: Create .claude/commands/ directory**
```bash
mkdir -p .claude/commands
```

**Step 2: Create a command**
```bash
cat > .claude/commands/test.sh << 'EOF'
#!/bin/bash
# Usage: /test
# Runs all tests and shows coverage

bun run test:all
bun run test:coverage
echo "✓ Tests passed"
EOF

chmod +x .claude/commands/test.sh
```

**Step 3: Use it**
```
> /test
# Claude runs all tests and shows results
```

**Useful Commands to Create:**

```bash
# /commit - Smart commit message
/commit "Build auth system" 
# → Claude writes good commit message, commits, pushes

# /deploy - Deploy to production
/deploy
# → Claude runs tests, builds, deploys, verifies

# /cleanup - Remove dead code
/cleanup
# → Claude finds unused variables/functions, removes them

# /optimize - Make code faster
/optimize
# → Claude profiles, finds bottlenecks, optimizes

# /docs - Generate documentation
/docs
# → Claude generates README, API docs, guides
```

### Real Example: Build Your Own

```bash
cat > .claude/commands/ship.sh << 'EOF'
#!/bin/bash
# Usage: /ship
# Prepare code for production

echo "Running tests..."
bun run test:all || exit 1

echo "Linting..."
bun run lint || exit 1

echo "Building..."
bun run build || exit 1

echo "Type: Ready to ship? (yes/no)"
read response

if [ "$response" = "yes" ]; then
  git push origin main
  echo "✓ Shipped to production"
else
  echo "Cancelled."
fi
EOF

chmod +x .claude/commands/ship.sh
```

Then just:
```
> /ship
# Does full pre-production checklist
# Commits, pushes when ready
```

---

## SECTION 12: EFFORT LEVELS (THINKING MODE)

**The Idea:** For hard problems, tell Claude to think longer.

### How-To: Use Effort Levels

**Step 1: Check current effort**
```bash
claude
> /effort
# Shows: Low, Medium, High, Max
```

**Step 2: Set effort for hard problems**
```
> /effort max

Now working on a hard architectural problem:
> "Redesign our data caching strategy"

# Claude spends more tokens, thinks longer, gives better answer
```

**When to Use Each:**

| Effort | Use Case |
|--------|----------|
| Low | Simple fixes, obvious tasks |
| Medium | Normal work (default) |
| High | Complex logic, design decisions |
| Max | Architecture overhauls, hard bugs |

**Real Example:**
```
/effort high
> "Design a system that handles 1 million concurrent users.
  Our current setup maxes at 100K. 
  What's the bottleneck? How do we fix it?"

Claude thinks deeply, explores options, gives detailed architecture.
Uses more tokens, but solution is solid.
```

---

## 🎓 PRACTICE ROADMAP: Become a Master

### Week 1: Fundamentals
- [ ] Day 1-2: Read this guide, understand Plan Mode
- [ ] Day 3-4: Try Plan Mode on a real project (plan, then build)
- [ ] Day 5-6: Create your first CLAUDE.md
- [ ] Day 7: Build something with Plan Mode + CLAUDE.md

**Deliverable:** Small project built with Plan Mode

### Week 2: Speed
- [ ] Day 8-9: Set up git worktrees, run 2 Claudes in parallel
- [ ] Day 10-11: Run 5 Claudes in parallel (need 5 worktrees)
- [ ] Day 12-13: Learn git merge (merge 5 parallel branches)
- [ ] Day 14: Build full feature with 5 parallel Claudes

**Deliverable:** Feature built 3x faster with parallel work

### Week 3: Quality
- [ ] Day 15-16: Set up verification (browser for web apps)
- [ ] Day 17-18: Create custom agents (CodeReviewer, ArchitectureAdvisor)
- [ ] Day 19-20: Set up hooks (auto-format, auto-lint)
- [ ] Day 21: Build feature with full verification pipeline

**Deliverable:** Production-ready feature built in 1 day

### Week 4: Mastery
- [ ] Day 22-23: Learn /batch (parallelize 10 subagents)
- [ ] Day 24-25: Create custom slash commands
- [ ] Day 26-27: Use voice dictation for all prompts
- [ ] Day 28: Build entire small app (all techniques combined)

**Deliverable:** Full app built with all techniques

---

## 🚀 COMPLETE EXAMPLE: Build a Real SaaS Feature

### Scenario: Building "Lead Scoring API" for NightPivot

**Setup:**
```bash
# Create worktrees
git worktree add .claude/wt1 origin/main  # Database
git worktree add .claude/wt2 origin/main  # API
git worktree add .claude/wt3 origin/main  # Tests + Docs
```

**Terminal 1: Database (30 min)**
```bash
cd .claude/wt1
claude

# Plan mode
Shift+Tab Shift+Tab

> "I need to design a database schema for lead scoring.
   Fields: lead_id, company_name, industry, revenue, pain_points, 
   interaction_count, last_contacted, ai_automation_fit_score.
   
   I'll show you the database structure, 
   you approve it, then build migrations."

# Claude shows plan
# You approve
# Claude builds schema + migrations
```

**Terminal 2: API (30 min)**
```bash
cd .claude/wt2
claude

> /effort high
> "Build a scoring API endpoint.
   Takes lead data, scores on 0-100 scale.
   Scoring rules:
   - Industry (SaaS +20, fintech +25, healthcare +15, other +5)
   - Company revenue (>$10M +20, >$1M +10)
   - Pain points mentioned (operations +30, data +20, customer service +20)
   - Interaction count (+5 per interaction, max +30)
   
   Return: { lead_id, score, breakdown, confidence, next_steps }
   
   Add tests for each scoring rule."

# Claude builds full API with tests
```

**Terminal 3: Tests + Docs**
```bash
cd .claude/wt3
claude

> "Write comprehensive tests for the scoring system.
   Test each scoring rule independently.
   Test edge cases (no data, invalid input).
   Generate API documentation (OpenAPI spec).
   Write a scoring guide for the sales team."

# Claude writes tests + docs
```

**Merge Results:**
```bash
git merge .claude/wt1  # Database goes first
git merge .claude/wt2  # API depends on database
git merge .claude/wt3  # Tests depend on both

# All done, feature is complete and tested
# Total time: ~2 hours
# Without parallel: ~6 hours
```

---

## 💡 The Meta-Lesson: Verification Loop

**Why Boris is so fast:**

```
Feedback Loop = Speed

❌ Slow: Code → Deploy → Feedback (1 day later)
✅ Fast: Code → Test Locally → Feedback (1 minute)
✅✅ Fastest: Code → Claude Tests Automatically → Feedback (instantly)

The fastest developers give Claude a verification loop.
Claude tests, finds issues, fixes, re-tests.
No human waiting, no deployment delays.
```

**Examples:**
- Web dev: Browser verification (Claude opens browser, tests, fixes)
- API: API testing (Claude tests endpoints, fixes issues)
- Database: SQL testing (Claude queries, verifies data integrity)
- Performance: Benchmarking (Claude profiles, optimizes)

---

## 📝 Quick Reference: Commands You'll Use Most

```bash
# Plan mode
Shift+Tab Shift+Tab              # Enter plan mode
Shift+Tab                         # Set expectations

# Parallel work
git worktree add .claude/wt1 main # New isolated workspace
git worktree remove .claude/wt1   # Remove worktree

# Custom agents
claude --agent=CodeReviewer       # Use custom agent

# Voice
/voice                            # Voice input mode
(Hold spacebar, talk, release)   # Speak your prompt

# Effort levels
/effort max                        # Think longer on hard problems

# Batch processing
/batch "Do X with 10 agents"     # Parallelize subagents

# Configure
/config                           # Settings
/terminal-setup                   # Fix terminal rendering
/vim                              # Enable vim mode
```

---

## 🎯 The Master's Mindset

After mastering these techniques, you think like this:

```
Task: "Build a complex feature"

Your thought process:
1. Can I parallelize this? 
   → Yes, split into 5 parts → spawn 5 Claudes
   
2. Do I need a plan or can I go straight to code?
   → It's complex → use Plan Mode
   
3. Can Claude verify the work?
   → Yes, it's a web feature → give browser verification
   
4. Do I have CLAUDE.md patterns for this?
   → Check CLAUDE.md first, follow those patterns
   
5. Should I use custom agents?
   → Yes, spawn CodeReviewer in parallel
   
6. Voice or typed?
   → Voice (3x faster, more detailed)

Result: Complex feature built in hours, not days.
Quality is high because verification is built-in.
```

---

## 📚 Summary: 12 Techniques Ranked by Impact

| Rank | Technique | Impact | Effort |
|------|-----------|--------|--------|
| 1 | Verification (Browser) | 3x quality | Low |
| 2 | Plan Mode | 2x speed | Low |
| 3 | Parallel Execution (5 Claudes) | 5x speed | Medium |
| 4 | CLAUDE.md | Compounding improvement | Low |
| 5 | Git Worktrees | Enables parallelization | Low |
| 6 | Custom Agents | Better specialized work | Medium |
| 7 | Voice Dictation | 3x faster prompting | Low |
| 8 | /batch | 10x speed on big tasks | Medium |
| 9 | Hooks (Auto-format, auto-test) | Always perfect code | Medium |
| 10 | Mobile Control | Work anywhere | Low |
| 11 | Slash Commands | Faster workflows | Low |
| 12 | Effort Levels | Better on hard problems | Low |

**Master these 3 first, then add others:**
1. Plan Mode (1-2 hours to learn)
2. Parallel work with worktrees (1-2 hours to learn)
3. CLAUDE.md (30 min to set up)

After 1 week of practice with these 3: **3-5x faster development**.

---

**You're ready. Start with Plan Mode on your next task. Come back to this guide as you need each technique.**

**Good luck, and welcome to the Claude Code master class! 🚀**
