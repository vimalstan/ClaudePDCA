# Debug

Systematic investigation of bugs and unexpected behavior.

## When to Use

Bug reports, test failures, unexpected behavior. Part of Plan phase: debug → rca → plan.

## Process

1. **Reproduce**: confirm the bug exists, get exact steps
2. **Isolate**: narrow to smallest failing case
3. **Hypothesize**: form 2-3 competing theories for root cause
4. **Test each**: verify/disprove each hypothesis with evidence
5. **Root Cause**: identify the actual cause (not symptoms)
6. **RCA document**: write root cause analysis
   - What happened
   - Why it happened
   - What to fix
   - How to prevent recurrence (test, rule, or lint)
7. Route to plan → build → check → ship

## Competing Hypotheses (multi-agent)

For hard bugs, spawn parallel agents with different theories. Each tries to disprove the others. Surviving theory is more reliable.

## Red Flags

- Jumping to a fix before understanding the cause
- Fixing symptoms instead of root cause
- Not adding a regression test
- Not adding a preventive rule/lint for the error class
