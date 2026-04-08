# Orchestrator

Stateless lifecycle router. Runs its own PDCA at every sub-phase boundary to decide what happens next.

## How It Works

At each sub-phase boundary:
1. Read `task-state.json` + `escalation-criteria.md` + latest verdict
2. Evaluate: right outcome? right skill? escalation needed?
3. Make ONE routing decision
4. Write updated `task-state.json`

The orchestrator holds no state between boundaries. `task-state.json` IS its memory.

## Routing Logic

### Task-Size Tiers

| Tier | Criteria | Process |
|---|---|---|
| Trivial | Typo, dep bump, config | build → check → ship |
| Small | Single-file fix, isolated | plan → build → check → ship |
| Medium | Multi-file, new endpoint | spec → plan → build → check → ship |
| Large | Cross-module, security-critical | full PDCA with adversarial |

### Skill Selection

Read `escalation-criteria.md` at each boundary. Key signals:

**Escalate to adversarial-build if ANY:**
- Cross-module integration (imports across 2+ domain boundaries)
- State machine, workflow orchestration, complex async
- Security-critical paths (auth, payments, encryption, PII)
- New public API contract
- Prior learnings flag this area as error-prone

**Stay with standard build if ALL:**
- Changes isolated to single domain boundary
- No security-critical paths
- No prior learnings flagging this area

**Multi-agent if ANY:**
- 3+ independent sub-tasks with clear file ownership
- Cross-layer work (frontend + backend + database) with no shared files

### Dynamic Switching

- Spec gaps during build → pause, return to spec
- New domain discovered → load specialist skill
- Build reveals need for prototype → pause, prototype, resume
- Review finds fundamental design flaw → return to plan

### Lens Selection (domain-based)

| Files touched | Lenses added |
|---|---|
| Always | correctness |
| auth, payments, encryption, PII | + security |
| repos, DB, queries, migrations | + performance |
| components, UI, CSS | + maintainability, edge-cases |
| API routes, controllers, shared types | + api-boundary |
| test files | + test-fidelity |
| cross-module imports | + maintainability |

Defined in `lens-routing.md`.

### Model Routing

| Phase | Model |
|---|---|
| Plan, Check, adversarial-build | Opus |
| Build, Setup, Ship | Sonnet |
| Trivial tier | Haiku |

## task-state.json Schema

```json
{
  "task_id": "string",
  "issue": "string (GitHub issue ref)",
  "tier": "trivial | small | medium | large",
  "current_phase": "Plan | Do | Check | Act",
  "current_sub_phase": "string",
  "increment": "number",
  "gate_results": ["string"],
  "escalation_history": ["string"],
  "EOF": "STATE_COMPLETE"
}
```

Missing EOF = invalid state file = halt and escalate.
