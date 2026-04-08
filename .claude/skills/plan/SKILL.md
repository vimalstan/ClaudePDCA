# Plan

Decompose spec into ordered implementation steps. Produces `plan.md`.

## When to Use

Small, Medium, and Large tier tasks. Skipped for Trivial.

## Process

1. Read `spec.md` (must exist for Medium+)
2. Break into ordered implementation increments
3. Each increment: what to build, which files, expected tests
4. Identify dependencies between increments (which must be sequential)
5. Identify parallelizable increments (for multi-agent work)
6. Declare file ownership per increment/agent
7. Estimate tier if not already set
8. Adversarial plan-check reviews the plan

## Output

`plan.md` in `.claude/` with sections:
- Increments (ordered, numbered)
  - Description
  - Files to create/modify
  - Tests to write
  - Dependencies (which increments must complete first)
- File Ownership Map (for parallel agents)
- Scope Boundaries (what's in, what's out)

## Gate

Plan-check must converge before proceeding to Do.

## Scope Decomposition

If the plan has multiple independent concerns:
- Each concern gets its own commit
- Destructive changes (deletions) are always the last commit
- Report decomposition to user before starting
