# Spec

Define what to build. Produces `spec.md` — the single source of truth for the task.

## When to Use

Medium and Large tier tasks. Skipped for Trivial and Small.

## Process

1. Gather requirements from user, issues, or conversation
2. Define functional requirements (what it does)
3. Define NFRs (performance targets, security requirements, accessibility, scalability)
4. Define acceptance criteria (ATDD — these become executable tests)
5. Define anti-patterns (approaches to avoid, with reasons)
6. Define constraints (tech stack, compatibility, dependencies)
7. Adversarial plan-check reviews the spec before proceeding

## Output

`spec.md` in `.claude/` with sections:
- Summary (one paragraph)
- Functional Requirements (numbered)
- Non-Functional Requirements (measurable)
- Acceptance Criteria (testable, becomes ATDD)
- Anti-Patterns (what NOT to do)
- Constraints
- Open Questions (if any — must be resolved before Do)

## Gate

Plan-check must converge (no Critical/High in 2 consecutive passes) before proceeding to Plan or Do.
