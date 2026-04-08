# TDD Build

Default build strategy. Red → Green → Refactor per increment.

## Process (per increment)

1. Write failing test(s) for this increment's expected behavior
2. Run tests — confirm they fail (red)
3. Write minimal implementation to make tests pass
4. Run tests — confirm they pass (green)
5. Refactor if needed — tests must stay green
6. Update scratchpad (files touched, decisions made)
7. Commit increment with conventional commit message

## Integration Tests

When connecting components (increment depends on prior increment):
- Write integration test covering the interaction
- Run full test suite, not just new tests

## Contract Tests

When creating/modifying API endpoints:
- Write Zod schema for request/response
- Verify frontend types match backend output shape
