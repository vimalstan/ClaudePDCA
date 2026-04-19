# Positioning — Review Gate Framework (retrospective)

> Branch: master (retroactive; body of work predates the gate being live — grandfathered per task-close Step 5c fallback, produced as good-faith audit)

## 1. Enclosing system summary

ClaudeWoW is a meta-engineering project defining PDCA workflow, skills, and quality gates for Claude-assisted work. This body of work adds Layer-0 positioning + review convergence gates on top of existing skills (red-team-review, check, adversarial-codegen, task-open, task-close), plus observability via audit hooks.

## 2. Upstream dependencies

| System | What I consume | Contract/SLA | Breaks me if... |
|---|---|---|---|
| `red-team-review` skill | Exit Criteria + Phase 3–5 (patched) | `Status: CONVERGED` + iteration floor | Criterion reverts |
| `positioning` skill (new) | 8-section schema, blast-radius output | tier labels Small/Medium/Large | Schema reshaped |
| `task-open` / `task-close` | Step 6.5, Step 5c new gates | Skill-text enforcement | Skills re-rewritten without these steps |
| Claude Code harness | PostToolUse + SessionStart hook support | Watcher reloads `.claude/` after `/hooks` | Harness drops hook support on Windows/git-bash |
| `.claude/rules/*.md` | Style, security, testing conventions | Read by task-open | Rules diverge from skill guidance |

## 3. Downstream consumers

| Consumer | What they read | Contract I expose | Breaks them if I change... |
|---|---|---|---|
| `check` skill | Convergence Definition (patched) | Requires red-team for Medium+ | Criterion reverts |
| `build/adversarial.md` | Convergence section (patched) | Same | Criterion reverts |
| `audit-commit.sh` | `.claude/artifacts/<branch>/*.md` existence | File paths stable | Artifact directory moves |
| `audit-alert.sh` | `.claude/artifacts/audit.log` format | Line format `<ts> commit=X branch=Y positioning=y/n redteam=y/n` | Log format changes |
| Future feature tasks | Gate requirement = part of workflow | Floor 3/5/7 by tier | Tier labels change |
| STD-013, STD-014 | Named patterns in red-team-review `standards.md` | Surfaced in future red-team passes | Standards renamed/removed |

## 4. Sibling features

- `.claude/rules/*.md` — project-scoped terse rules. Cited, not duplicated.
- `.claude/skills/*` (spec, plan, build, check, ship, docs, debug, parallel, rollback, setup, orchestrator, bootstrap, frontend-ui, prototype) — existing PDCA skills. Gates integrate with these; no merger.
- `.claude/patterns/README.md` — patterns registry scaffold (sibling created in this body of work; empty until rule-of-three triggers).

## 5. Precedent scan (evidence-cited)

- `Glob C:/Users/Vimal/.claude/skills/**/*.md` — ~40 skill files; reviewed relevant ones (red-team-review, task-open, task-close, adversarial-codegen).
- `Glob c:/Users/Vimal/Documents/Work/ClaudeWoW/.claude/skills/**/*` — 30 project skills.
- Previous convergence criterion at `red-team-review/SKILL.md:178-185` (replaced in this work).
- Existing `rules/*.md` as model for `.claude/rules/` convention (terse, authoritative).
- `adversarial-codegen/SKILL.md` — reviewed; different architecture (6 parallel prosecutors + defender); intentionally not patched.

Precedent judgments: extended existing rules rather than creating parallel `principles/` dir; copied shell style from existing SessionStart hook.

## 6. Abstraction-level decision

**Platform** (cross-project, home-directory-scoped) for the skill/standards work — shared across every project. Component-scoped for ClaudeWoW-internal changes (check patch, adversarial patch, patterns README).

Rationale:
- `~/.claude/skills/` changes apply to every project, not just ClaudeWoW → Platform discipline
- Backward-compatible: only adds sections; existing invocation patterns unchanged
- STD-013 / STD-014 are append-only with Origin field → history preserved

## 7. Alternatives considered

| Alternative | Rejected because |
|---|---|
| Skill-text-only enforcement (no audit hook) | Bypass-able by skipping task-open; no observability |
| Hard PreToolUse gate blocking `git commit` | Too restrictive for emergency commits |
| Replicate adversarial-codegen architecture (6 prosecutors) across all reviews | Pathological cost for small tasks; overkill for simple edits |
| `principles/` registry parallel to `rules/` | Single-source-of-truth violation; extend existing rules instead |
| Positioning embedded in brainstorming skill | Wrong abstraction level; positioning runs before brainstorm |

## 8. Blast radius

**Large** — 5+ skills touched across `~/.claude/` and `ClaudeWoW/.claude/`, affects every future implementation task, harness-level hooks active. Triggers Large floor of 7 iterations in red-team-review. Met in practice — see `red-team.md`.
