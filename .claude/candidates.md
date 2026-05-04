# Candidates

Pending patterns awaiting promotion. TTL: 5 task-close cycles without promotion → pruned.

## C-001: Contract-critical small task

- **First seen:** 2026-05-02 — red-team of `~/.claude/hooks/model-recommend.sh` (3-iteration pass; dogfooding gap surfaced in Iter-3, recorded in `~/.claude/artifacts/model-recommend-hook/red-team.md`).
- **Shape:** Single-file change where size suggests Sonnet but silent-failure risk justifies Opus. Wrong protocol/contract output (hook JSON schema, MCP message shape, header format) produces no error at write-time — the feature just silently doesn't work in production.
- **Gap in current task-shape categories:** SessionStart hook classifies on size only (Opus = architecture/multi-file, Sonnet = single-feature, Haiku = trivial). No axis for failure-mode visibility. A 30-line shell script with a contract assumption is a different risk class than a 30-line component.
- **Likely promotion path:** rule update — either amend the SessionStart hook prose to add a "contract-critical → Opus regardless of size" clause, OR add `.claude/rules/model-selection.md` cited from the hook. Not a code abstraction (`patterns/README.md` "anti-pattern" carve-out applies: missing-guardrail, not missing-shape).
- **Promote when:** 2nd occurrence observed — another small-by-size task where silent-failure-on-contract-mismatch was the dominant risk.
- **TTL:** 5 task-close cycles from 2026-05-02. Prune if no 2nd occurrence by then.
