# Escalation Criteria

Living document. Agent proposes changes via candidates. Human approves promotion.

## Build Escalation

**Escalate to adversarial-build if ANY:**
- Plan describes cross-module integration (imports across 2+ domain boundaries)
- Task involves state machine, workflow orchestration, or complex async coordination
- Changes touch security-critical paths (auth, payments, encryption, PII handling)
- New public API contract consumed by external clients
- Prior learnings flag this area as error-prone
- Changes to middleware/request pipeline ordering
- Changes to data access scoping mechanisms or tenant resolution logic
- Changes to how security-critical services are composed, wired, or made available to consuming code

**Stay with standard build if ALL:**
- Changes isolated to a single domain boundary
- No security-critical paths involved
- Bug fix with isolated, confirmed root cause
- No prior learnings flagging this area

## Mid-Build Escalation

- Do-check flags unanticipated coupling or complexity → remaining increments get adversarial treatment, no restart

## Insufficient Information

- Plan lacks detail to evaluate criteria → pause, return to Plan for refinement

## Multi-Agent Escalation

- 3+ independent sub-tasks with clear file ownership boundaries
- Cross-layer work (frontend + backend + database) with no shared files
- Parallelizable research/investigation

## General Skill Switching

- Spec gaps found during build → pause, return to spec
- New domain discovered → load specialist skill
- Build reveals need for prototype → pause, prototype, resume
- Review finds fundamental design flaw → return to plan
