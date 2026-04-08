# Build

Write code that passes tests. Routes to the appropriate build strategy.

## Routing

Read `plan.md` and `escalation-criteria.md`:
- Standard build → load `tdd.md`
- Complex/security-critical build → load `adversarial.md`

Orchestrator may switch mid-build if do-check flags unanticipated complexity.

## Do-Check Frequency

- Standard build: milestone boundaries (defined in plan)
- Adversarial build: every increment

## Do-Check Concerns

1. **Plan drift**: is the implementation following the plan?
2. **Increment soundness**: is each increment solid before the next?

## Gate

Do-check verdict written by review subagent (never the builder). JSON with EOF marker. Partial writes rejected.
