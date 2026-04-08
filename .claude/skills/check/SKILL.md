# Check

Adversarial review with convergence. Verify + review merged into one phase.

## Process

```
repeat (max 10 iterations):
  1. Load lenses (domain-based, from lens-routing.md)
  2. Run each lens against the code
  3. Run freeform adversarial thinking
  4. Classify findings: known pattern or new
  5. New patterns → gates/{task-id}/candidates.json (with hash)
  6. Rate severity
  7. If no Critical/High in 2 consecutive passes → CONVERGED
  8. Else → fix findings, loop
Circuit breaker at iteration 8 → escalate with remaining findings
```

## Lenses

Loaded from `lenses/` directory. Each lens is a separate file (OCP — add without modifying this skill).

Domain determines which lenses run (see `orchestrator/lens-routing.md`).

## Gate Transaction

Review subagent produces:
1. `gates/{task-id}/verdict.json` (findings + status)
2. `gates/{task-id}/candidates.json` (new patterns, with finding hash)
3. EOF marker ("VERDICT_COMPLETE")

All three required. Missing any = invalid gate = blocked.

## Convergence Definition

No Critical or High findings in 2 consecutive iterations.

## Post-Convergence

Merge task-specific candidates.json into global candidates.md (lead agent, sequential).

## Kaizen

After check, evaluate: did the right lenses run? Log routing improvements as candidates.
