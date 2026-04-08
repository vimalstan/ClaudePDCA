# Ship

Publish verified code. Only runs after Check converges.

## Process

1. Verify all tests pass (final run)
2. Review unpromoted candidates — promote / reject / defer
3. Commit with conventional commit message
4. Push branch
5. Create PR via `gh pr create` (link to GitHub issue)
6. Update GitHub Project board (move issue to Done)
7. Merge to main (after PR approval)
8. Tag merge: `release/YYYY-MM-DD-{branch-name}` (for rollback reference)
9. Update CLAUDE.md if significant change
10. Extract learnings from scratchpad → basic-memory
11. Delete scratchpad, task-state.json, gates/{task-id}/
12. Clean up worktree if used

## Ship Failure

One retry allowed:
1. Ship → fail (CI red, merge conflict)
2. Diagnose root cause
3. Fix
4. Re-ship
5. If retry fails → escalate

## Fast-Track Override

For trivial/hotfix changes only:
- Explicit — must be stated
- Audited — noted in PR description
- No gate files produced
- Skip does not mean skip tests — tests always run
