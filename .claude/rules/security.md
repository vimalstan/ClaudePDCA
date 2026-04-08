# Security Rules

- Never commit secrets (.env, API keys, tokens, credentials)
- Validate all external input at system boundaries (user input, API payloads)
- Use parameterized queries — never string-interpolate SQL
- Sanitize output to prevent XSS
- Use httpOnly cookies for session tokens, never localStorage
- Enforce tenant isolation at the data access layer
- Log security events, never log secrets or PII
- OWASP top 10 awareness on every endpoint
