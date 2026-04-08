# Getting Started with ClaudeWoW

## What This Is

A structured way of working with Claude Code that produces high-quality, secure, maintainable software. Instead of ad-hoc prompting, you get a repeatable PDCA workflow with enforcement, testing, and continuous improvement built in.

## 1. Setup (5 minutes)

```bash
# Clone the skeleton
git clone <this-repo> claude-wow
cd claude-wow

# Copy into your project (or use as-is for a new project)
cp -r .claude/ /path/to/your/project/.claude/
cp -r base/ /path/to/your/project/base/
cp .gitignore /path/to/your/project/
cp .env.example /path/to/your/project/
cp settings.json.template /path/to/your/project/.claude/settings.json
cp -r .github/ /path/to/your/project/.github/
```

Or tell Claude: **"Bootstrap this project using the ClaudeWoW skeleton."**

## 2. Customize (10 minutes)

1. **CLAUDE.md** — Copy `CLAUDE.md.template` to `CLAUDE.md` in your project root. Fill in:
   - Project name and description
   - Tech stack
   - Domain boundaries (these drive file ownership for parallel agents)
   - Conventions
   - NFR baselines

2. **Design system** — The `base/` design system works out of the box. Override only what's different:
   - Need custom brand colors? Copy `base/tokens/colors.md` to your project, modify it
   - Happy with default spacing? Leave it — the base is used automatically

3. **Settings** — Edit `.claude/settings.json`:
   - Replace `<project-path>` with your actual path
   - Adjust model preferences if needed

## 3. Start Working

Tell Claude what you want to build. The orchestrator handles the rest.

### Example prompts by task size:

**Trivial** (config change, typo fix):
```
Fix the typo in the login page heading — it says "Welcom" instead of "Welcome"
```
→ Claude routes: build → check → ship

**Small** (isolated fix):
```
The /api/users endpoint returns 500 when the user has no profile picture. Fix it.
```
→ Claude routes: plan → build → check → ship

**Medium** (new feature):
```
Add a forgot password flow. Users should receive a reset link via email, 
valid for 1 hour. The reset page should validate the token and let them 
set a new password.
```
→ Claude routes: spec → plan → build → check → ship

**Large** (cross-module, security-critical):
```
Add role-based access control. Support 4 roles: admin, creator, reviewer, 
assessee. Each role has specific permissions for viewing, creating, and 
managing assessments. Enforce at both API and UI level.
```
→ Claude routes: spec → prototype → plan → setup → adversarial build → check → ship

### You don't need to specify the tier

The orchestrator evaluates your task and picks the right process. If it escalates mid-build (e.g., discovers unexpected complexity), it tells you why.

## 4. What Happens Behind the Scenes

```
You: "Add forgot password flow"
  │
  ├── Orchestrator: evaluates → Medium tier
  │
  ├── Spec: writes spec.md with requirements, NFRs, acceptance criteria
  │   └── Plan-check: adversarial review of the spec → converges
  │
  ├── Plan: decomposes into increments with file ownership
  │   └── Plan-check: reviews the plan → converges
  │
  ├── Setup: creates branch, runs baseline tests, creates scratchpad
  │
  ├── Build (per increment):
  │   ├── Write failing tests (TDD)
  │   ├── Implement until green
  │   ├── Do-check: review subagent verifies increment
  │   └── Record learnings
  │
  ├── Check: full adversarial review (correctness + domain lenses)
  │   └── Converges: no Critical/High in 2 consecutive passes
  │
  └── Ship: commit, PR, tag, update docs, clean up
```

## 5. Key Concepts

### PDCA (Plan-Do-Check-Act)
Every phase has an adversarial review gate. Nothing proceeds until the gate converges.

### Convergence
"No Critical or High findings in 2 consecutive review iterations." If iteration 8 is reached without convergence, Claude escalates to you with the remaining issues.

### Enforcement, Not Discipline
Gate verdicts are written by a separate review subagent — the builder can't mark its own work as done. JSON files with EOF markers reject partial writes. Hooks validate file ownership.

### Kaizen (Continuous Improvement)
Every review generates learnings. New patterns go to `candidates.md`. Patterns that prove valuable get promoted to `standards.md` over time. Review lenses, escalation criteria, and routing rules all improve automatically.

### Scratchpad
Working memory for the current task. Survives context compactions. Automatically created at task start, deleted at task completion after learnings are extracted.

## 6. Multi-Agent Work

For large tasks, Claude can spin up parallel agents:

```
Add user management: CRUD endpoints, admin UI, role assignment, 
and audit logging. These are independent — parallelize.
```

Each agent gets its own worktree and branch. No two agents edit the same file. The lead agent owns shared files (package.json, config).

## 7. When Things Go Wrong

**Bug in production?** Tell Claude: "Roll back the last release"
→ Rollback skill assesses safety (code, DB, infra), reverts safe layers, escalates destructive changes

**Build not converging?** Claude escalates at iteration 8 with the specific disagreements. You decide.

**Wrong approach?** The orchestrator re-evaluates at every sub-phase boundary. If the approach isn't working, it switches skills automatically.

## 8. Files You'll Interact With

| File | You write | Claude writes | Purpose |
|---|---|---|---|
| `CLAUDE.md` | Initial setup | Updates after changes | Project constitution |
| `.claude/standards.md` | Approve promotions | Proposes candidates | Review patterns |
| `.claude/learnings/index.md` | Review occasionally | Maintains | Accumulated knowledge |
| `.claude/scratchpad.md` | Read if curious | Full lifecycle | Current task state |
| `base/tokens/*.md` | Override if needed | Never | Design system |
| GitHub Projects board | Add issues | Updates status | Work tracking |

## 9. Tips

- **Start small.** Run a few Small tier tasks first to see the workflow in action before attempting a Large.
- **Trust the tiers.** Don't force full PDCA on a typo fix. The orchestrator picks the right ceremony level.
- **Review learnings monthly.** Open Obsidian (or `.claude/learnings/index.md`), see what Claude has learned. Promote good patterns, prune bad ones.
- **Conventional commits matter.** They drive automated versioning. `feat:` = minor bump, `fix:` = patch, `BREAKING CHANGE:` = major.
- **Let check run.** The adversarial review catches real issues. Resist the urge to fast-track.
