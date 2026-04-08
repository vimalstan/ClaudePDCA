# Prototype

Visualize before building. Produces throwaway artifacts that inform the plan.

## When to Use

Orchestrator invokes based on task type:
- UI/UX feature with user-facing flows → load `ui.md`
- Complex architecture, 3+ service/component boundaries → load `architecture.md`
- Simple CRUD / bug fix / backend logic → skip

## Output

Throwaway artifacts in `.claude/prototypes/` (gitignored). Not the deliverable — informs the plan.

## Gate

No formal gate. Prototype informs spec/plan refinement.
