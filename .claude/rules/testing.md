# Testing Rules

- TDD: write failing tests before implementation — red, green, refactor
- ATDD: acceptance tests derived from spec before coding starts
- Run baseline tests before starting any task — know what's already broken
- Every migration classified as reversible or destructive at check phase
- Contract tests via shared Zod schemas for frontend-backend alignment
- Never mock the thing you're testing
- Integration tests hit real services, not mocks
- Tests must be deterministic — no time-dependent or order-dependent tests
- Data-aggregation integration tests (dashboards, reports, list views, batch operations) MUST assert expected query counts
- Query count budgets are set when the test is written — adjustments require explicit justification in the commit message
