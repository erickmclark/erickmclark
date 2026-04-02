# Boris Cherny's Complete Claude Code Guide
## All Historical Tweets & Tips for Using Claude Code Better

**Compiled:** April 1, 2026  
**Source:** Boris Cherny (@bcherny) — Creator of Claude Code at Anthropic  
**Coverage:** 72+ tips from January 2026 - March 2026

---

## Table of Contents
1. [Core Philosophy](#core-philosophy)
2. [Parallel Execution & Sessions](#parallel-execution--sessions)
3. [Model Selection & Effort](#model-selection--effort)
4. [Plan Mode Mastery](#plan-mode-mastery)
5. [CLAUDE.md - Self-Training Your Agent](#claudemd---self-training-your-agent)
6. [Skills & Slash Commands](#skills--slash-commands)
7. [Subagents & Agents](#subagents--agents)
8. [Hooks & Automation](#hooks--automation)
9. [Permissions & Safety](#permissions--safety)
10. [MCP Integrations](#mcp-integrations)
11. [Terminal Setup & Customization](#terminal-setup--customization)
12. [Verification & Testing](#verification--testing)
13. [Code Review & Quality](#code-review--quality)
14. [Mobile & Remote Control](#mobile--remote-control)
15. [Advanced Features](#advanced-features)
16. [Hidden Gems](#hidden-gems)

---

## Core Philosophy

### The Foundational Principle
**Boris's #1 tip:** "Probably the most important thing to get great results out of Claude Code — give Claude a way to verify its work. If Claude has that feedback loop, it will 2-3x the quality of the final result."

Think of it like asking someone to build a website but they aren't allowed to use a browser — the result probably won't look good. Give Claude a browser and it will write code and iterate until it looks good.

### No Single "Right Way"
> "Claude Code works great out of the box, so I personally don't customize it much. There is no one correct way to use Claude Code: we intentionally build it in a way that you can use it, customize it, and hack it however you like."

The team uses Claude differently than Boris does. **Experiment to see what works for you.**

### Building for Future Models
> "At Anthropic, we don't build for the model of today, we build for the model of six months from now. And that's still my advice to founders that are building on LLMs."

---

## Parallel Execution & Sessions

### Run 5+ Claude Sessions Simultaneously

Boris runs **5 instances of Claude Code simultaneously** in his terminal using 5 separate git checkouts of the same repo.

```bash
~/repo-1 $ claude
~/repo-2 $ claude
~/repo-3 $ claude
~/repo-4 $ claude
~/repo-5 $ claude
```

**Setup tips:**
- Number your tabs 1-5 for easy reference
- Configure iTerm2 notifications to know when any Claude needs attention
- Each tab runs in its own git checkout, so Claude can make changes in parallel without conflicts

### Web + Local Sessions

Beyond the terminal, Boris runs **5-10 additional sessions on claude.ai/code**.

**Commands:**
- `&` — Background a session
- `--teleport` — Switch contexts between local and web
- Hand off fluidly between local and web

**Mobile workflow:**
- Kick off sessions from your phone via the Claude iOS app in the morning
- Pick them up on your computer later

---

## Model Selection & Effort

### Use Opus 4.5 (or Latest) with Thinking Mode

**Boris's choice:** Opus 4.5 with thinking mode for every task.

**Why Opus over Sonnet:**
> "It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end."

**The takeaway:** Less steering + better tool use = faster overall results, even with a larger model.

### Effort Levels

Run `/model` to pick your preferred effort level:

- **Low** — Less tokens & faster responses
- **Medium** — Balanced behavior
- **High** — More tokens & more intelligence (Boris's default)
- **Max** — For deep reasoning on hard problems (reasoning mode)

Use `--effort max` when you need Claude to think longer and use more tokens — useful for hard debugging, architecture decisions, or tricky code where you want Claude to really think it through.

---

## Plan Mode Mastery

### Start with the Plan

Most sessions start in **Plan mode** (`shift+tab` twice).

**The workflow:**
1. Describe what you want
2. Iterate on the plan with Claude until it's solid
3. Switch to auto-accept mode
4. Claude 1-shots the implementation

**Example:**
```
> i want to improve progress notification rendering for skills. 
> can you make it look and feel a bit more like subagent progress?

▮▮ plan mode on (shift+tab to cycle)
```

**Best practice:** "A good plan is really important to avoid issues down the line."

### When Things Go Sideways

**Don't keep pushing** — switch back to plan mode and re-plan.

**Team patterns:**
- One person has one Claude write the plan, then spin up a second Claude to review it as a staff engineer
- The moment something goes sideways, switch back to plan mode and re-plan
- Explicitly tell Claude to enter plan mode for verification steps, not just for the build

### Plan for Verification Too

Pour your energy into the plan so Claude can 1-shot the implementation. Include verification steps in the plan itself:

```
> Try "refactor cli.tsx"

▮▮ plan mode on (shift+Tab to cycle)
```

---

## CLAUDE.md - Self-Training Your Agent

### The Team's Secret Weapon

The Claude Code team shares a **single CLAUDE.md file** for the repo, checked into git. The whole team contributes multiple times a week.

**Key practice:**
> "Anytime we see Claude do something incorrectly we add it to the CLAUDE.md, so Claude knows not to do it next time."

**Example CLAUDE.md:**
```
**Always use `bun`, not `npm`.**

bun run typecheck
bun run test -- -t "test name"
bun run test:file -- "glob"
bun run lint:file -- "file1.ts"
bun run lint
bun run lint:claude && bun run test
```

### The PR Workflow: Compounding Engineering

During code review, tag `@.claude` on PRs to add learnings to CLAUDE.md as part of the PR itself.

**Example:**
```
nit: use a string literal, not ts enum

@claude add to CLAUDE.md to never use enums,
always prefer literal unions
```

**Result:** Claude automatically updates CLAUDE.md and commits:
```
Prefer `type` over `interface`; 
**never use `enum`** (use string literal unions instead)
```

This is their version of **"Compounding Engineering"** — inspired by Dan Shipper's concept.

### After Every Correction

End with: **"Update your CLAUDE.md so you don't make that mistake again."**

Claude is eerily good at writing rules for itself.

**Pro tip:** Ruthlessly edit your CLAUDE.md over time. Keep iterating until Claude's mistake rate measurably drops.

### Advanced Pattern: Multi-File Tracking

One engineer tells Claude to maintain a notes directory for every task/project, updated after every PR. They then point CLAUDE.md at it:

```
└ ~/.claude/CLAUDE.md: 76 tokens
└ CLAUDE.md: 4k tokens
```

---

## Skills & Slash Commands

### Turn Repeated Tasks into Skills

**Rule of thumb:** If you do something more than once a day, turn it into a skill or slash command.

**Commands are checked into git** under `.claude/commands/` and shared with the team.

**Example:**
```
> /commit-push-pr

/commit-push-pr Commit, push, and open a PR
```

**Power feature:** Slash commands can include inline Bash to pre-compute info (like git status) for quick execution without extra model calls.

### Team Skill Examples

- **`/techdebt`** — Run at the end of every session to find and kill duplicated code
- **`/sync-context`** — Syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump
- **`/analyze-metrics`** — Pull and analyze metrics on the fly using BigQuery

### Built-In Power Commands

- **`/simplify`** — Use parallel agents to improve code quality, tune efficiency, and ensure CLAUDE.md compliance
- **`/batch`** — Fan out work to parallel agents running with full isolation using git worktrees
- **`/loop`** — Schedule Claude to run automatically at a set interval (up to 3 days locally)
- **`/schedule`** — Create cloud-based recurring jobs (runs even when laptop is closed)
- **`/btw`** — Ask quick questions mid-task without interrupting Claude's work
- **`/branch`** — Fork an existing session
- **`/effort`** — Set reasoning effort level for current session
- **`/color`** — Change prompt input color to distinguish parallel sessions

---

## Subagents & Agents

### Think of Subagents as Automations

Subagents are for the most common PR workflows:

```
▼ .claude
▼ agents
├─ build-validator.md
├─ code-architect.md
├─ code-simplifier.md
├─ oncall-guide.md
└─ verify-app.md
```

**Examples:**
- **`code-simplifier`** — Cleans up code after Claude finishes
- **`verify-app`** — Detailed instructions for end-to-end testing

### Using Subagents Strategically

> "use 5 subagents"

Append **"use subagents"** to any request where you want Claude to throw more compute at the problem:

```
> use 5 subagents to explore the codebase

● I'll launch 5 explore agents in parallel to...
● Running 5 Explore agents... (ctrl+o to expand)
├─ Explore entry points and startup
├─ Explore React components structure
├─ Explore tools implementation
├─ Explore state management
└─ Explore testing infrastructure
```

**Use cases:**
- Offload individual tasks to keep your main agent's context window clean and focused
- Route permission requests to Opus 4.5 via a hook — let it scan for attacks and auto-approve safe ones

### Custom Agents with Restricted Tools

Custom agents are often overlooked. Define them in `.claude/agents/`:

```bash
$ cat .claude/agents/ReadOnly.md
---
name: ReadOnly
description: Read-only agent restricted to the Read tool only
color: blue
tools: Read
---

You are a read-only agent that cannot edit files or run bash.

$ claude --agent=ReadOnly
```

**Features:**
- Custom name, color, tool set, and system prompt
- Great for read-only agents, specialized review agents, or domain-specific tools

### Auto-Approval for Subagents

Route permission requests to Opus via a hook — let it scan for attacks and auto-approve the safe ones.

---

## Hooks & Automation

### Core Hook Events

Hooks let you deterministically run logic as part of the agent lifecycle:

**Common patterns:**

1. **SessionStart** — Dynamically load context each time you start Claude
2. **PreToolUse** — Log every bash command the model runs
3. **PostToolUse** — Auto-format code after edits
4. **PermissionRequest** — Route permission prompts to WhatsApp/Slack for approval/denial
5. **Stop** — Poke Claude to keep going whenever it stops
6. **PostCompact** — Fire after Claude compresses conversation context

### Example: Auto-Format on Edit

```json
{
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
  ]
}
```

### Example: PostCompact Hook

Fires after Claude compresses context, letting you inject instructions or run commands when compaction happens:

```json
{
  "hooks": {
    "PostCompact": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Context was compacted'"
          }
        ]
      }
    ]
  }
}
```

### Long-Running Task Hooks

For very long-running tasks, ensure Claude can work uninterrupted:

**Options:**
- (a) Prompt Claude to verify with a background agent when done
- (b) Use a Stop hook for deterministic checks
- (c) Use the "ralph-wiggum" plugin (community idea)

For sandboxed environments: `--permission-mode=dontAsk` or `--dangerously-skip-permissions` to avoid blocks.

---

## Permissions & Safety

### Pre-Allow Safe Commands

Instead of `--dangerously-skip-permissions`, use `/permissions` to pre-allow common safe commands. Most are shared in `.claude/settings.json`.

Run `/permissions` to customize:

```
Permissions: [Allow] Ask Deny

52. Bash(gh issue view:*)
53. Bash(gh pr checks:*)
54. Bash(gh pr comment:*)
55. Bash(gh pr diff:*)
56. Bash(gh pr list:*)
```

**Wildcard syntax supported:**
- `Bash(bun run *)`
- `Edit(/docs/**)`

### Auto Mode (No More Permission Prompts)

Instead of approving every file write and bash command:

```bash
$ claude --enable-auto-mode
```

**How it works:** Anthropic built and tested classifiers that evaluate each action before it runs:
- Safe operations (reading files, running tests) → auto-approved
- Risky ones (deleting files, force-pushing, unknown scripts) → still flagged

**Boris's take:** "no 👏 more 👏 permission 👏 prompts 👏"

### Sandbox Runtime

Opt into Claude Code's open source sandbox runtime to improve safety while reducing permission prompts:

```bash
$ /sandbox
```

**Sandbox modes:**
- Sandbox BashTool with auto-allow — commands run in sandbox, auto-approved
- Sandbox BashTool with regular permissions — sandbox + normal prompts
- No Sandbox — default behavior

---

## MCP Integrations

### Enable MCPs in .claude/settings.json

Claude Code uses all of Boris's tools autonomously:

```json
{
  "mcpServers": {
    "slack": {
      "type": "http",
      "url": "https://slack.mcp.anthropic.com/mcp"
    }
  }
}
```

### Common MCP Use Cases

1. **Slack MCP** — Search and post to Slack
   ```
   > Go fix the failing CI tests
   > Or: fix this https://ant.slack.com/archives/...
   ```

2. **BigQuery** — Run analytics queries directly
   - Boris's team has a BigQuery skill checked into the codebase
   - "Personally, I haven't written a line of SQL in 6+ months."

3. **Sentry** — Grab error logs from Sentry

4. **GitHub Actions** — Check CI status, run workflows

5. **iMessage Plugin** (New!)
   ```bash
   $ /plugin install imessage@claude-plugins-official
   ```
   - Text Claude like you'd text a friend
   - Works from iPhone, iPad, or Mac

### Docker Logs for Distributed Systems

Point Claude at docker logs to troubleshoot distributed systems — it's surprisingly capable.

---

## Terminal Setup & Customization

### Essential Settings

Run `/config` to set:

- **Theme** — Light/dark mode
- **Notifications** — Enable for iTerm2 or custom hooks
- **Newlines** — If using IDE terminal, Apple Terminal, Warp, or Alacritty: `run /terminal-setup`
- **Vim mode** — Run `/vim`

### Terminal Recommendations

**Ghostty** — Multiple team members recommend it:
- Synchronized rendering
- 24-bit color
- Proper unicode support

### Customization Power Features

**Status line** — Show context usage and current git branch:
```bash
$ /statusline

[Opus] 📁 my-app | 🌿 feature/auth
█████████░░░░░░░░░ 42% | $0.08 | 🕐 7m 3s
```

**Custom keybindings** — Every key binding is customizable:
```bash
$ /keybindings
```

**Spinner verbs** — Make it personal. Ask Claude to customize to Star Trek theme:
```
✱ Beaming up… (esc to interrupt)
```

**Output styles** — Run `/config` and set a style:
- **Explanatory** — Great when getting familiar with new codebase
- **Learning** — Have Claude coach you through changes
- **Custom** — Create your own voice

### Voice Dictation

**Fun fact:** Boris does most of his coding by speaking to Claude, rather than typing.

- **CLI:** Run `/voice` then hold space bar
- **Desktop:** Press the voice button
- **iOS:** Enable dictation in settings

**Why it works:** You speak 3x faster than you type, and your prompts get way more detailed as a result.

---

## Verification & Testing

### The #1 Rule: Give Claude a Way to Verify

This is Boris's most important tip. The verification method varies by domain:

- **Web code** — Use the Chrome extension to open a browser, test UI changes, iterate
- **Backend** — Bash commands, test suites, simulators
- **Systems** — Docker logs, distributed system debugging
- **Mobile** — Device simulators

**Takeaway:** Invest in domain-specific verification for optimal performance.

### Desktop App Auto-Testing

The Claude Desktop app can:
- Automatically run your web server
- Test it in a built-in browser
- Iterate until it works perfectly

**Alternatives:**
- Use Chrome extension in CLI or VSCode
- Or just use the Desktop app for integrated experience

### Test Suite Strategy

For the Claude Code repo itself:
- Claude tests every single change Boris lands
- Tests run automatically before commits
- Browser testing with Chrome extension for UI changes

---

## Code Review & Quality

### Automated PR Review Team

When a PR opens, Claude dispatches a team of agents to hunt for bugs. Anthropic built it for themselves first — **code output per engineer is up 200% this year**.

**What it does:**
- Automatically reviews every PR with a team of agents
- Each agent focuses on different concerns: logic errors, security issues, performance regressions
- Posts inline comments directly on the PR

**Real results:** The system catches real bugs Boris wouldn't have noticed otherwise.

### Challenge Claude on Quality

Don't accept the first solution. Three strategies:

**a) Make Claude be your reviewer**
```
> Grill me on these changes and don't make a PR until I pass your test.
```

**b) Compare before/after**
```
> Prove to me this works. Diff behavior between main and your feature branch.
```

**c) Demand the elegant solution**
```
> Knowing everything you know now, scrap this and implement the elegant solution.
```

**Key insight:** Don't accept the first solution. Push Claude to do better — it usually can.

### Detailed Specs = Better Output

Write detailed specs and reduce ambiguity before handing work off. The more specific you are, the better the output.

---

## Mobile & Remote Control

### Claude Code Mobile App

**Did you know Claude Code has a mobile app?** Boris writes a lot of his code from the iOS app — it's a convenient way to make changes without opening a laptop.

**Setup:**
- Download Claude app for iOS/Android
- Navigate to the Code tab
- You can review changes, approve PRs, and write code directly from your phone

### Teleporting Between Devices

Move sessions back and forth between mobile/web/desktop and terminal:

```bash
$ claude --teleport
```

Or from within a session:
```
> /teleport
> /remote-control
```

**Boris's setup:** "I have 'Enable Remote Control for all sessions' set in my /config."

### Remote Control Dispatch

**Cowork Dispatch** — A secure remote control for the Claude Desktop app.

> "When I'm not coding, I'm dispatching."

**What it does:**
- Can use your MCPs, browser, and computer with your permission
- Way to delegate non-coding tasks to Claude from anywhere
- Boris uses it every day to catch up on Slack and emails, manage files

---

## Advanced Features

### Git Worktrees (The Productivity Unlock)

Run **3-5 git worktrees at once**, each running its own Claude session in parallel. **It's the single biggest productivity unlock.**

```bash
$ git worktree add .claude/worktrees/my-worktree origin/main
$ cd .claude/worktrees/my-worktree && claude
```

Or use the Desktop app — check the worktree checkbox.

**Why worktrees over checkouts:**
- Most of the team prefers worktrees
- Reason @amorriscode built native support into Claude Desktop
- Some people name worktrees and set up shell aliases (za, zb, zc) to hop between them in one keystroke
- Dedicated "analysis" worktree for just reading logs and running BigQuery

**With subagents:**
```
> Migrate all sync io to async. Batch up the changes,
> and launch 10 parallel agents with worktree isolation.
> Make sure each agent tests its changes end to end,
> then have it put up a PR.
```

### Worktree Isolation for Subagents

Make subagents always run in their own worktree:

```yaml
---
name: worktree-worker
model: haiku
isolation: worktree
---
```

### Non-Git VCS Support

For Mercurial, Perforce, or SVN: define worktree hooks to benefit from isolation without Git.

```json
{
  "hooks": {
    "WorktreeCreate": [
      { "command": "jj workspace add \\\"$(cat /dev/stdin | jq -r '.name')\\\"" }
    ],
    "WorktreeRemove": [
      { "command": "jj workspace forget \\\"$(cat /dev/stdin | jq -r '.worktree_path')\\\"" }
    ]
  }
}
```

### Scheduled Tasks: /loop vs /schedule

**`/loop`** — Local recurring (up to 3 days):
```bash
$ /loop 5m /babysit
$ /loop 30m /slack-feedback
$ /loop 1h /pr-pruner
```

**`/schedule`** — Cloud-based recurring (unlimited, even when laptop closed):
```
> /schedule a daily job that looks at all PRs shipped
> since yesterday and update our docs based on the changes.
```

**Common use cases:**
- Auto-address code review, auto-rebase, shepherd PRs to production
- Automatically put up PRs for Slack feedback every 30 mins
- Address code review comments you missed
- Close out stale PRs

---

## Hidden Gems

### The /bare Flag (10x Faster Startup)

By default, when you run `claude -p` (or the TypeScript/Python SDKs), Claude searches for local CLAUDE.md's, settings, and MCPs.

For non-interactive usage, use `--bare`:

```bash
$ claude -p "summarize this codebase" \
  --output-format=stream-json \
  --verbose \
  --bare
```

**Why it matters:** 10x faster startup. This was a design oversight when the SDK was first built. In a future version, they'll flip the default to `--bare`.

### Working Across Multiple Repos

```bash
$ claude --add-dir /path/to/other-repo
```

Or within a session:
```
> /add-dir /path/to/other-repo
```

**Team setup:** Add `"additionalDirectories"` to your team's settings.json to always load additional folders.

### Session Management

**Branch a session:**
```bash
$ /branch
$ claude --resume <session-id> --fork-session
```

**Name your session at launch:**
```bash
$ claude --name "auth-refactor"
```

**Auto-naming after plan mode:**
```
> shift+tab
[plan]> Refactor the auth middleware to use JWT
```
Claude infers a descriptive session name.

### Plugins & Marketplaces

Run `/plugin` to browse and install plugins:
- LSPs (now available for every major language)
- MCPs
- Skills
- Custom hooks

Install from the official Anthropic marketplace, or create your own marketplace for your company. Check settings.json into your codebase to auto-add marketplaces for your team.

### 37 Settings & 84 Environment Variables

Pick a behavior, and it's likely configurable. Store config in `config.json`. Ask the user on first run if it's missing. Use the "env" field to avoid wrapper scripts.

---

## Using Claude Code for Learning

### Three Learning Strategies

**a) Enable "Explanatory" output style**
```bash
$ /config
```
Have Claude explain the why behind its changes.

**b) Generate visual HTML presentations**
```
> Generate a visual HTML presentation explaining unfamiliar code.
```

**c) ASCII diagrams of protocols and codebases**
```
> Draw ASCII diagrams of new protocols and codebases to help me understand them.
```

**d) Build a spaced-repetition learning skill**
```
You explain your understanding → Claude asks follow-ups → stores the result
```

**Key takeaway:** Claude Code isn't just for writing code — it's a powerful learning tool when you configure it to explain and teach.

---

## Configuration as Code

### Check Settings Into Git

Claude Code is built to work great out of the box. When you do customize, **check your settings.json into git** so your team can benefit:

**Configuration levels:**
- For your codebase
- For a sub-folder
- For just yourself
- Via enterprise-wide policies

**Total configuration surface:**
- 37 settings
- 84 environment variables (use "env" field in settings.json to avoid wrapper scripts)

---

## Spotify Case Study: Real Impact

> "Love seeing how Spotify is shipping with Claude Code. Their best developers haven't written a single line of code since December, they fix bugs from their phones, and they shipped 50+ features from Slack during morning commutes."

This captures the potential: **code authorship through voice dictation, mobile apps, remote control, and autonomous loops.**

---

## Key Quotes from Boris

1. **On verification:** "Give Claude a way to verify its work. If Claude has that feedback loop, it will 2-3x the quality."

2. **On models:** "It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end."

3. **On automation:** "If you do something more than once a day, turn it into a skill or command."

4. **On permissions:** "No more permission prompts."

5. **On remote work:** "When I'm not coding, I'm dispatching."

6. **On customization:** "We intentionally build it in a way that you can use it, customize it, and hack it however you like."

7. **On skill building:** "Ruthlessly edit your CLAUDE.md over time. Keep iterating until Claude's mistake rate measurably drops."

---

## Actionable Next Steps to Improve Your Claude Code Usage

### Immediate (This Session)
- [ ] Start in **Plan mode** (`shift+tab` twice) before building
- [ ] Turn on **auto mode** (`--enable-auto-mode`) to eliminate permission prompts
- [ ] Enable Chrome extension for web work verification
- [ ] Create a basic `.claude/CLAUDE.md` with 3-5 rules you know Claude breaks

### This Week
- [ ] Set up **3-5 git worktrees** and run Claude in parallel
- [ ] Create your first slash command (e.g., `/commit-push-pr`)
- [ ] Configure **PostToolUse hook** for auto-formatting
- [ ] Enable **iMessage plugin** to text Claude from phone
- [ ] Set up **voice dictation** and try it for 1 prompt

### This Month
- [ ] Build a **subagent team** for your most common PR workflows
- [ ] Create 5-10 custom skills for repeated tasks
- [ ] Set up **/loop** for recurring CI babysitting or Slack summaries
- [ ] Configure **desktop app** to auto-run and test web servers
- [ ] Review and optimize your **permissions** via `/permissions`

### Long-Term (Compounding Engineering)
- [ ] Make **CLAUDE.md** your team's source of truth
- [ ] Tag `@.claude` in PRs to build institutional knowledge
- [ ] Use **Cowork Dispatch** for non-coding tasks from anywhere
- [ ] Set up cloud-based **/schedule** jobs for autonomous work
- [ ] Measure and iterate on Claude's performance per improvement

---

**Document compiled April 1, 2026**  
**All tips sourced from Boris Cherny's public tweets January - March 2026**  
**For latest updates: https://howborisusesclaudecode.com**
