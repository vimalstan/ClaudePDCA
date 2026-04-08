# Setup

Prepare the environment for a task. Runs at the start of every Do phase.

## Process

1. Check git state (branch, uncommitted changes, stashes, stale worktrees)
2. Create worktree + branch (backend/risky) or direct branch (frontend/UI)
   - Worktree: `.worktrees/{branch-name}`, ensure `.worktrees/` is gitignored
   - Direct: `git checkout -b type/short-description origin/main`
   - Branch naming: feat/, fix/, refactor/, chore/, docs/
3. Install dependencies (auto-detect: package.json, requirements.txt, pubspec.yaml)
4. Read relevant docs (CLAUDE.md, domain-specific conventions)
5. Run baseline tests — confirm green before starting
   - If tests fail: STOP, flag to user, do not proceed
6. Create GitHub issue if not already tracked
7. Create scratchpad: `.claude/scratchpad.md` with task template
8. Initialize task-state.json

## Scratchpad Template

```markdown
# Scratchpad: [task name]
Branch: [branch name]
Worktree: [path or "direct"]
Issue: [#number]
Created: [date]

## Task
[one-line description]

## Status
In progress

## Progress
- [ ] Pending steps from plan

## Decisions
(none yet)

## Learnings
(none yet)

## Files Touched
(none yet)

## Next Steps
(from plan)
```

## Red Flags

- Coding on main
- Skipping baseline tests
- Using worktree for frontend work (breaks Vite live preview)
- Starting without reading conventions
