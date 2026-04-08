# File Structure Rules

- One concern per file (new code; existing refactored opportunistically)
- Barrel files (index.ts) re-export only — no logic (enforce via lint)
- Per-domain type files, not one shared types index
- Routes: per-domain, auto-discovered via glob pattern
- Migrations: timestamped, one change per migration file
- Feature folders for cohesion, files within for SRP
  e.g., user/user.model.ts, user/user.service.ts, user/user.controller.ts
- Group small utilities by domain concern, not one-file-per-function
