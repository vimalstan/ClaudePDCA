# Security Lens

- Injection vectors (SQL, command, XSS)?
- Auth bypass or privilege escalation?
- Tenant data leakage or cross-tenant access?
- Data exposure in logs, errors, or responses?
- OWASP top 10 applicability?
- Secrets hardcoded or leaked into version control?
- Input validation at system boundaries?
- Security/compliance dependencies typed as optional? If optional, is there a fail-closed runtime check (throw on null, never skip)?
- Middleware/pipeline execution order covered by an integration test? Any ordering change since last review?
