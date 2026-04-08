# Parallel

Coordinate multiple agents working on the same task.

## When to Use

Orchestrator determines: 3+ independent sub-tasks with clear file ownership boundaries.

## Architecture

```
.claude/coordination/
  task-list.json        ← shared, file-locked on claim
  messages/{agent}.json ← per-agent inbox
  ownership.json        ← file ownership map
```

## Rules

- File ownership declared in plan, enforced by TaskCreated hook
- No concurrent writes to the same file — ever
- Shared files (package.json, config) owned by lead agent only
- Tasks sharing write targets are sequential, not parallel
- Each agent: own worktree + own branch

## Coordination Pattern

1. Lead decomposes plan into independent tasks
2. Lead writes ownership.json (file → agent mapping)
3. Agents claim tasks from task-list.json (file-locked)
4. Agents work in isolated worktrees
5. Agents request shared file changes via messages to lead
6. Lead applies shared file changes sequentially
7. Each agent merges (rebase onto main) when their task passes check
8. Lead resolves any conflicts

## Cross-Agent Dependencies

Use Zod contracts. Agent-A defines API shape in spec. Agent-B builds against the contract. Contract testing catches divergence.
