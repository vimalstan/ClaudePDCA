# Docs

Generate documentation from existing artifacts. Routes to appropriate doc type.

## Sub-Types

- `release-notes.md` — changelog + user-facing notes from commits/PRs
- `adr.md` — architecture decision records
- `api-docs.md` — API documentation from code

## When to Use

- Release train invokes release-notes automatically
- ADRs written when significant architectural decisions are made
- API docs generated when public API changes
