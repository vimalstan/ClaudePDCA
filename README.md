# Claude Way of Work (ClaudeWoW)

A PDCA-based development workflow skeleton for Claude Code. Provides structured skills, enforcement mechanisms, and continuous improvement for building high-quality software with AI agent swarms.

## Quick Start

1. Clone this repo
2. Copy contents into your project directory
3. Run the `bootstrap` skill: tell Claude "bootstrap this project"
4. Customize `CLAUDE.md` for your project

## Architecture

### PDCA Lifecycle

```
Plan → Do → Check → Act
```

- **Plan** (composable): spec, prototype, plan, debug, rca, research
- **Do**: setup → [build increment → do-check] per increment
- **Check**: adversarial review with convergence (max 10 iterations)
- **Act**: ship or escalate

### Task-Size Tiers

| Tier | Process |
|---|---|
| Trivial | build → check → ship |
| Small | plan → build → check → ship |
| Medium | spec → plan → build → check → ship |
| Large | full PDCA with adversarial build |

### Skills (14)

| Category | Skills |
|---|---|
| Lifecycle | spec, prototype, plan, setup, build, check, ship |
| Specialist | frontend-ui, debug, parallel, docs |
| Operational | orchestrator, bootstrap, rollback |

### Rules (7 always-on)

security, git, testing, style, memory, file-structure, dependencies

### Enforcement

- Gate verdicts written by review subagent (not the builder)
- JSON with EOF marker — partial writes rejected
- Convergence: no Critical/High in 2 consecutive iterations
- Circuit breaker at iteration 8 → auto-escalate

### Testing

- **Per-task**: TDD (unit), ATDD (acceptance), integration, contract (Zod)
- **Release train**: E2E, performance, security, visual regression (GitHub Actions)

### Memory

- **CodeSight MCP**: codebase context (AST-based, local)
- **basic-memory MCP**: agent memory (explicit writes, local, git-tracked)
- **Obsidian**: human curation layer (vault root = .claude/)
- **.claude/**: single source of truth for all project knowledge

### Multi-Agent

- Each agent: own worktree + own branch
- No concurrent writes to same file
- File ownership declared in plan, enforced by hooks
- Coordination via `.claude/coordination/`

## Directory Structure

```
.claude/
├── rules/                    7 always-on rules
├── skills/                   14 skills (lifecycle + specialist + operational)
│   ├── orchestrator/         Stateless lifecycle router
│   ├── spec/                 Define what to build
│   ├── prototype/            Visualize before building
│   ├── plan/                 Decompose into steps
│   ├── setup/                Prepare environment
│   ├── build/                TDD or adversarial build
│   ├── check/                Adversarial review + lenses
│   ├── ship/                 Publish + cleanup
│   ├── bootstrap/            One-time project init
│   ├── rollback/             Undo production damage
│   ├── docs/                 Release notes, ADRs, API docs
│   ├── debug/                Systematic bug investigation
│   ├── parallel/             Multi-agent coordination
│   └── frontend-ui/          UI specialist
├── learnings/                Phase-scoped wiki-style learnings
├── coordination/             Multi-agent task list + messaging
├── standards.md              Promoted review patterns
├── candidates.md             Pending patterns
└── lens-routing.md           → in orchestrator/

base/
├── tokens/                   Design system defaults (colors, typography, spacing)
└── components/               Component specs (button, input, card)

.github/workflows/
└── release-train.yml         Scheduled test + release pipeline
```

## Design Principles

- **SOLID**: Single responsibility skills, open for extension (lenses), dependency inversion (file-based communication)
- **KISS**: Task-size tiers — lighter process for lighter tasks
- **DRY**: One review mechanism, one convergence pattern, one learning flow
- **PDCA**: Plan-Do-Check-Act at every level (lifecycle, orchestrator, learning)
- **Enforcement over discipline**: Gate files, hooks, subagent verdicts — never rely on prompting alone
- **Kaizen**: Continuous improvement of lenses, criteria, and standards

## License

MIT
