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
  "status": "running | completed | error",
  "started_at": "ISO 8601 timestamp",
  "updated_at": "ISO 8601 timestamp",
  "completed_at": "ISO 8601 timestamp or null",
  "gate_results": ["string"],
  "escalation_history": ["string"],
  "EOF": "STATE_COMPLETE"
}
```

### Validation Rules

- Missing EOF = invalid state file = halt and escalate
- `status: running` + `updated_at` older than 60 minutes = stale task, likely crashed → orchestrator flags for recovery
- `status: error` = previous run failed → orchestrator presents error context and asks whether to retry or escalate

### Recovery

On SessionStart, if task-state.json exists:
1. Check status — if `running` + stale → mark `error`, surface to user
2. If `error` → offer: retry from last completed gate, or escalate
3. If `completed` → leftover from prior task, clean up

## Delta Pattern for Learnings

Learnings index and detail files use append-only writes during a task. Full compaction runs during first-session-of-day cleanup:
- Append: new entries added to end of file (cheap, no full rewrite)
- Compact: deduplicate, remove pruned candidates, rebuild index (periodic, cleanup job)
