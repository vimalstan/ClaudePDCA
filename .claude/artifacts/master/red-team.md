# Red Team Review — Review Gate Framework (retrospective)

**Status:** CONVERGED

**Iteration count:** 9 (meets Large floor of 7)

**Audiences:** user (reviewer/approver), me (drafting agent), future sessions, subagents, audit consumers (user reading `audit.log`)

## Iteration history

Each iteration was driven by user pushback (the most adversarial hostile persona available) — not by self-attestation.

| # | Trigger | Tactic | Finding(s) | Resolution |
|---|---|---|---|---|
| 1 | Initial framework design | Structured lenses (freeform) | 5-layer scaffold drafted; plausible-but-ungrounded | Initial artifact produced |
| 2 | User: "SOLID, DRY, KISS, ..." | User-surfaced principles coverage | Registry scope too narrow | Expanded cross-cutting principle layer |
| 3 | User: `/red-team-review` | Self-applied lens sweep | Converged at iteration 2 — WRONG (premature exit) | Declared "converged" — caught in next iteration |
| 4 | User: "convergence not the goal, quality is; laziness unacceptable" | Hostile-persona pass (senior engineer cold review) | Convergence-criterion-is-lazy → **STD-013**; confidence-false-signal | Replaced exit criterion with adversarial-pressure-exhausted |
| 5 | User: "wider picture" | Hostile-persona pass (system architect) | Layer-0 positioning missing → **STD-014** | Added positioning skill + layer |
| 6 | User: "go" (apply to red-team-review) | TDD-for-skills RED/GREEN subagent pair | Iron Law violated (edited skill without RED first) — reverted | Applied TDD properly; patched skill |
| 7 | User: "parallelise" (streams A–D) | Future-failure inventory (replicate adversarial-codegen?) | Different architecture; tier-based dispatch fits better | Kept 3 review skills; added dispatch rule |
| 8 | User: "how will you find if gate is bypassed?" | Hostile-persona pass (audit engineer) | Skill-text alone is bypass-able; no observability | Audit-log hook + SessionStart alert |
| 9 | User: "parallelise" (apply hooks) | TDD + pipe-test + node JSON validation + dogfooding | Hooks verified; master-filter bug in alert script surfaced | Fixed; re-tested; retrospective positioning+red-team produced |

## Phase coverage

- **Phase 0 — Pre-flight:** audiences/entities/integration/taxonomy enumerated, refined at iteration 5.
- **Phase 1 — Structured lenses:** correctness, security, performance, maintainability, edge cases, test fidelity, API boundary — applied at each subagent spawn.
- **Phase 2 — Freeform adversarial:** every iteration.
- **Phase 3 — Hostile persona:** senior engineer cold, system architect, audit engineer — rotated.
- **Phase 4 — Future-failure inventory (≥5 disposed):**
  1. Framework ships without enforcement hooks → audit-log hook
  2. Grounding becomes box-ticking → tier-scaled depth + evidence requirements
  3. `asserted`/`to-be-built` become laziness vectors → ratio cap + done-contract mapping
  4. Corpus staleness masks regression → flagged, deferred (no corpus yet)
  5. Framework's own quality ceiling self-attested → user-pushback-as-iteration + subagent GREEN tests
- **Phase 5 — Dogfooding:** framework applied to itself (this artifact); TDD-for-skills applied to red-team-review and task-open/close edits; writing-skills Iron Law caught violating the skill itself and reverted.

## Surviving residual risks

| Risk | Disposition |
|---|---|
| Audit log relies on user reviewing SessionStart alerts | **Accepted** — solo dev; escalate to hard gate if audit shows persistent bypass |
| Hook watcher may require `/hooks` reload on first session | **Accepted** — documented; user runs `/hooks` once |
| Task-id = branch name may collide across worktrees | **Accepted** — branch names unique per worktree |
| Positioning ritual friction for mechanical changes | **Mitigated** — skill has explicit skip clause |
| Patterns registry (`.claude/patterns/`) currently empty | **Accepted** — populates via Kaizen loop as patterns emerge |
| `adversarial-codegen` skill not patched | **Accepted** — different architecture has built-in anti-laziness; flagged for future TDD if empirical bypass surfaces |
| Framework predates its own gate (this conversation is grandfathered) | **Accepted** — this retrospective artifact is the audit |
| No pressure-scenario RED test for `check` / `adversarial` beyond the simple RED (check RED didn't reproduce lazy exit; adversarial textual parallel) | **Accepted** — patched defensively; audit log will surface if patch insufficient |

## Evidence of active attack in final iteration

Iteration 9 surfaced a real bug (master-branch alert noise in `audit-alert.sh`) that was fixed in-place, plus a JSON-validator gap (jq not installed on Windows) addressed by switching to node. Final iteration produced real findings — not passive re-reading.

## Unconverged items

None. All findings are resolved or explicitly accepted with reason.
