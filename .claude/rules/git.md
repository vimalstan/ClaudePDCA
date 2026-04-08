# Git Rules

- Branch before code — never commit directly to main
- Conventional commits enforced: `type(scope): description`
  - Types: feat, fix, refactor, chore, docs, perf, test
  - `BREAKING CHANGE:` footer for major version bumps
- One concern per commit — features, fixes, and deletions are separate commits
- Destructive changes (file deletions, dead code removal) are always the last commit
- Never force push to main/master
- Never amend published commits
- Never skip hooks (--no-verify)
- Worktree per backend/risky task, direct branch for frontend/UI
- No concurrent writes to the same file across agents — ever
