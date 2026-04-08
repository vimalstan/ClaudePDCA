# Rollback

Undo production damage. Revert first, investigate later.

## When to Use

- CI red after merge
- Smoke test failure after deploy
- User-reported bug traced to recent change

## Process

1. **Identify scope**: code only? + DB migration? + infra change?

2. **Assess safety per layer**:

   | Layer | Reversible | Action |
   |---|---|---|
   | Code | Always | git revert to release tag |
   | DB (add column, add table) | Yes | Run down migration |
   | DB (drop column, alter data) | No | STOP — escalate, do NOT auto-revert |
   | Infra (config change) | Yes | Revert IaC |
   | Infra (stateful: resize, DNS) | No | STOP — escalate |

3. **Execute rollback** (safe layers only):
   - DB down migration FIRST (if reversible)
   - Then code revert to release tag
   - Then infra revert (if stateless)

4. **Post-rollback**:
   - Create bug issue in GitHub Projects (link to reverted commit)
   - Reopen original task
   - Route to: debug → rca → plan → build → check → ship

## Rules

- Never auto-rollback destructive DB changes or stateful infra
- Always escalate to human for destructive rollbacks
- Every migration is classified reversible/destructive at check phase
- Ship skill tags every merge: `release/vX.Y.Z` for rollback reference
