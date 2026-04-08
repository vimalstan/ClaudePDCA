# Dependency Rules

- Dependabot or Renovate enabled on all repos (configured by bootstrap)
- Patch updates: auto-merge if release train tests green
- Minor updates: auto-merge if tests green + no breaking changelog
- Major updates: manual review — Small tier task (plan → build → check → ship)
- Security patches: auto-merge immediately regardless of version bump
- Pin exact versions in production dependencies
- Use lockfiles (package-lock.json, pubspec.lock, etc.) — always commit them
