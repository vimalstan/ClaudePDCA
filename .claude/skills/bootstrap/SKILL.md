# Bootstrap

One-time project initialization. Run once when starting a new project from the skeleton.

## Idempotent Guard

Check: does `.claude/skills/` already exist with content? If yes → abort: "Project already initialized."

## Process

1. Copy skeleton structure into project directory
2. Detect git provider (GitHub / GitLab / other)
3. Initialize git repo if not already
4. If GitHub:
   a. Create repo via `gh repo create`
   b. Create GitHub Project board (kanban: Backlog → Todo → In Progress → Done)
   c. Copy release-train.yml to `.github/workflows/`
   d. Enable Dependabot/Renovate
5. Install MCP servers:
   a. CodeSight — verify local operation
   b. basic-memory — **verify embeddings run locally** (blocking prerequisite)
6. Analyze project: detect tech stack, structure, conventions
7. Generate CLAUDE.md from analysis (user customizes after)
8. Create `.env.example` from project requirements
9. Run CodeSight first scan (generates `.codesight/`)
10. Configure git hooks:
    a. commit-msg: conventional commit validation
    b. pre-commit: gitleaks / secret scanning
11. Initial commit

## Post-Bootstrap

- User customizes CLAUDE.md (domain boundaries, conventions, NFRs)
- User overrides design system modules if needed (copy base → project)
- User creates Obsidian vault pointing at `.claude/`
