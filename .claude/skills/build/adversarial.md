# Adversarial Build

Contract-first build for complex, security-critical, or cross-module code.

## Process

### 1. Architect (contract generation)
- Read spec, plan, existing code, conventions
- Produce structural contract: names, interfaces, invariants, boundaries
- Present contract to user for approval

### 2. Builder (implementation)
- Reads contract ONLY — never sees review findings directly
- Follows TDD within contract scope (red → green → refactor)
- Optimizes for code quality, not minimum diff

### 3. Adversarial Review (do-check, every increment)
- Review subagent applies domain-relevant lenses
- Produces verdict with findings
- Builder never sees findings — only refined contract from step 1

### 4. Convergence

**Convergence is not the goal; artifact quality is.** "No more findings" signals attack stopped, not quality ceiling reached.

Exit requires ALL of:
- No Critical/High findings in 2 consecutive passes, AND
- Last pass cites the adversarial angle that produced its findings — passive re-reading does not count, AND
- For Medium+ blast radius artifacts: red-team-review's full exit criteria met (hostile-persona pass, future-failure inventory, minimum iteration floor).

Surviving findings → Architect refines contract → Builder rebuilds. Circuit breaker at iteration 8 → escalate. Passive Minor-only passes don't count as convergence.

## Key Principle

The Builder is the agent that drifts. The Architect and review subagent bracket it. The contract prevents drift. Findings refine the contract, not the code directly.
