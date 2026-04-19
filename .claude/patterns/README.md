# Patterns Registry

Recurring shapes in this codebase — tracked so they get abstracted before they become drift.

## Rule of Three

- **1st occurrence** of a shape: inline, no entry here
- **2nd occurrence**: add an entry here marked `emerging`
- **3rd occurrence**: **abstract before proceeding**. Create the shared primitive (module, component, helper). Update existing occurrences to use it. Mark entry here as `abstracted`.

Not following this compounds — 4 copies is worse than 3, and unifying 4 costs more than unifying 3. Day-2 abstraction is 10× day-1 abstraction.

## Adding an Entry

Create `<pattern-name>.md` in this directory with frontmatter:

```markdown
---
status: emerging | abstracted | deprecated
first_seen: <file:line>
current_occurrences: <count>
shape: <1-line description>
---

# <Pattern Name>

## When to recognize
Triggers — what does this shape look like? Why would you notice it?

## Current occurrences
- `<file:line>` — <context>
- `<file:line>` — <context>

## Proposed abstraction (status=emerging)
Sketch of the shared primitive — module name, interface, where it would live.

## Abstraction (status=abstracted)
Where the shared primitive lives + how to use it.

## Do NOT use when
Anti-application — cases where the pattern would be wrong.
```

## Kaizen Loop

At task-close, ask: **"What pattern did I just use that isn't in the registry?"**

If 2+ recent tasks hit the same unrecorded shape, add it as `emerging`. Patterns surface through use, not foresight — the registry grows from observed recurrence, not speculation.

Review the registry periodically: are `emerging` entries still at 2 occurrences, or have they quietly grown to 3+ without being flagged? Promote to `abstracted`, or deprecate if the "pattern" turned out to be a false signal (context-dependent, not actually repeating).

## Integration with `positioning`

The [positioning](../skills/positioning/SKILL.md) skill's **Precedent scan** checks this registry first, then greps the codebase. A registered pattern short-circuits the grep. If grep surfaces a pattern that isn't here, add it.

## When it's an anti-pattern, not a pattern

If the same **bug class** recurs 3 times, the problem is a missing guardrail — not a missing abstraction. Add a lint rule, test, hook, or rule in [`.claude/rules/`](../rules/) rather than an abstraction here.

Examples:
- 3 N+1 queries → add a query-count budget rule (already in testing.md), not a "QueryHelper" abstraction
- 3 unvalidated inputs → add a boundary-validation middleware + lint rule, not an "InputHandler" abstraction
- 3 silent-failure async generators → fix the generator idiom (see STD-005), not abstract the generator pattern

## Status legend

- **emerging** — 2 occurrences, abstraction pending. Blocks on 3rd use.
- **abstracted** — shared primitive exists, all occurrences use it. Maintenance only.
- **deprecated** — pattern was retired; entry retained as history so it's not re-introduced.
