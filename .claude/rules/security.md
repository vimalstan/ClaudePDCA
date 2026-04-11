# Security Rules

- Never commit secrets (.env, API keys, tokens, credentials)
- Validate all external input at system boundaries (user input, API payloads)
- Use parameterized queries — never string-interpolate SQL
- Sanitize output to prevent XSS
- Use httpOnly cookies for session tokens, never localStorage
- Enforce tenant isolation at the data access layer
- Log security events, never log secrets or PII
- OWASP top 10 awareness on every endpoint
- Security and compliance dependencies are fail-closed — if null, unavailable, or not injected, the operation MUST throw, never skip silently
- Prefer non-optional types for security dependencies so missing injection is a compile/type-check error, not a runtime surprise
- Middleware/pipeline execution order is a tested invariant — document expected order, verify with an integration test
