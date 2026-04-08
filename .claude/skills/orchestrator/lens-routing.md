# Lens Routing

Maps file domains to review lenses. Living document — self-improving via candidate flow.

## Routing Rules

| Files touched | Lenses added |
|---|---|
| Always (every review) | correctness |
| `**/auth/**`, `**/payment/**`, `**/encrypt**`, PII handlers | + security |
| `**/*.repo.*`, `**/migration*`, `**/*.query.*`, DB schemas | + performance |
| `**/components/**`, `**/*.css`, `**/*.scss`, UI files | + maintainability, edge-cases |
| `**/routes/**`, `**/controllers/**`, `**/shared/types/**` | + api-boundary |
| `**/*.test.*`, `**/*.spec.*`, `**/tests/**` | + test-fidelity |
| Cross-module imports (2+ domain boundaries) | + maintainability |

## Kaizen

- Lens addition: automated via candidate flow (finding with no matching lens → routing candidate)
- Lens removal: human only, never automated
- No findings 5+ consecutive tasks → investigate (review effectiveness or code quality), record learning
