# Example CLAUDE.md — Multi-Tenant Project

Reference for setting up a multi-tenant project with ClaudePDCA. Read this, then incorporate relevant sections into your project's own CLAUDE.md. This file is NOT loaded by the orchestrator — it is guidance only.

Sections marked **[adapt]** require project-specific values. Sections marked **[use as-is]** can be copied directly.

---

## Access Control Model [adapt]

Define the tenancy hierarchy. Every project has different levels; the example below uses three.

```
Platform Admin  — sees all tenants, manages system config
  Tenant Admin  — sees one tenant, manages orgs within it
    Org Admin   — sees one org within a tenant, manages users within it
```

For each level, declare what it can access:

| Role | Sees own tenant | Sees other tenants | Sees all orgs in tenant | Sees own org only |
|------|----------------|--------------------|------------------------|-------------------|
| Platform Admin | yes | yes | yes | n/a |
| Tenant Admin | yes | no | yes | n/a |
| Org Admin | yes | no | no | yes |

**Rules:**
- The hierarchy determines the query scope for every data access operation. No code should access data outside its caller's level.
- When two roles share a permission name but operate at different levels (e.g., both can "manage users" but at different scopes), the boundary is at the level, not the permission. Permissions describe capabilities; levels describe scope. These are different axes.
- Error responses MUST NOT leak resource existence across boundaries. A request for a resource outside the caller's scope returns 404 (not found), not 403 (forbidden). 403 confirms the resource exists.

---

## Multi-Tenancy Conventions [use as-is]

These apply to every multi-tenant project regardless of stack.

- Every table that holds tenant-scoped data has a tenant identifier column. Non-nullable. No exceptions.
- Tenant isolation is enforced at the data access layer structurally — not by convention. Prefer database-level enforcement (row-level security, views, connection-per-tenant) over application-level filtering. If application-level filtering is the only option, it must be in a single, auditable data access layer that all queries pass through.
- Missing tenant context is a hard failure. If a request reaches the data layer without an authenticated, verified tenant identifier, the operation throws — it does not pass through unscoped.
- Cross-tenant queries are explicit, audited, and rare. They use a dedicated method (e.g., `cross_tenant_query()`) that is distinct from normal data access, logs every invocation, and requires documented justification in the code.
- Nested resources verify ownership up the full chain. A request to `/tenants/A/orgs/B/users/C` must verify that org B belongs to tenant A AND that user C belongs to org B. Checking only the leaf resource is insufficient — this is how isolation bypasses happen (see Known Failure Modes, #1).
- All security and compliance dependencies are fail-closed. If a required service, repository, or middleware is null or not wired, the operation throws. Never skip silently. Prefer non-optional types so missing injection is a compile/type-check error (see Known Failure Modes, #4).
- Public endpoints that access tenant-scoped data resolve tenant context from the request (URL, signed token) rather than from user authentication — but tenant context must still be verified and present before data access
- Broad-scope queries by platform-level roles are normal operations, not "cross-tenant queries" — cross-tenant refers to a tenant-scoped user accessing data outside their scope

---

## Project-Local Lens: Multi-Tenancy [adapt]

Add this file to your project's `.claude/skills/check/lenses/multi-tenancy.md`. It will be applied during the Check phase for relevant files.

```markdown
# Multi-Tenancy Lens

- New tables: does every tenant-scoped table have a non-nullable tenant identifier column?
- Data access isolation: are all queries routed through the scoped data access layer? Any direct database access outside it?
- Nested resources: does the endpoint verify ownership up the full chain (tenant → org → resource), not just the leaf?
- Optional security dependencies: are any security/compliance services typed as optional or nullable? If so, is there a fail-closed runtime check?
- Silent skip patterns: any `if dependency is None: return` or `if not service: pass` for security operations? These must throw, not skip.
- Permission-as-role-proxy: any permission name used as a routing gate or visibility toggle that could apply to multiple roles at different scope levels?
- Error responses: do 403 responses leak resource existence across tenant boundaries? Should be 404.
- Cross-tenant access: any query that touches data across tenant boundaries without using the explicit cross-tenant method?
```

---

## Lens Routing Update [adapt]

Add the multi-tenancy lens to your project's `.claude/skills/orchestrator/lens-routing.md`:

```markdown
| `**/repos/**`, `**/repositories/**`, `**/dal/**`, data access layer | + multi-tenancy |
| `**/migrations/**`, `**/models/**`, DB schemas | + multi-tenancy |
| `**/middleware/**`, `**/pipeline/**`, request handling | + multi-tenancy |
| Cross-tenant query files, admin endpoints | + multi-tenancy |
```

This ensures the multi-tenancy lens is applied whenever the check phase reviews files that could affect tenant isolation.

---

## Escalation Criteria Update [adapt]

Add these project-specific adversarial triggers to your `.claude/skills/orchestrator/escalation-criteria.md`, under "Escalate to adversarial-build if ANY":

```markdown
- New entity/table creation (must have tenant ID and isolation policy)
- Changes to tenant resolution or tenant context propagation
- Changes to data access scoping or the query filtering layer
- New or modified cross-tenant query methods
- Changes to permission definitions or role-permission mappings
```

These are high-risk change categories where LLM-generated code has demonstrated failure patterns in multi-tenant systems.

---

## Testing Requirements [use as-is]

### Tenant Isolation Tests

Every entity that holds tenant-scoped data MUST have tenant isolation tests in the same commit that creates the entity. These are not optional and not deferred.

**Required test matrix per entity:**

| Scenario | Expected result |
|----------|----------------|
| Read own-tenant resource | 200 (success) |
| Read sibling-tenant resource (same parent, different org) | 404 |
| Read unrelated-tenant resource (different tenant entirely) | 404 |
| Create resource in own tenant | 201 (success) |
| Create resource in another tenant | 404 or 400 |
| List resources — only own-scope returned | Assert count and IDs match own scope exactly |

**Why 404, not 403:** A 403 response confirms the resource exists. Cross-boundary requests must return 404 as if the resource does not exist.

### Nested Ownership Chain Tests

For resources with parent-child relationships (e.g., tenant → org → user → assessment):

| Scenario | Expected result |
|----------|----------------|
| Access child through correct parent chain | 200 |
| Access child through wrong parent (child exists but parent is wrong) | 404 |
| Access child with missing intermediate parent | 404 |

### Query Budget Tests

Data-aggregation operations (dashboards, reports, list views) must include query count assertions. These catch N+1 regressions and ensure tenant scoping filters don't cause query explosion across tenant boundaries.

---

## Known Failure Modes [use as-is]

Real-world failure patterns from a multi-tenant SaaS project. Each pattern includes what happened, why it was missed, and what the review lens should look for.

### 1. Nested Resource Isolation Bypass

**What happened:** 25+ nested sub-resource endpoints checked resource existence without verifying org ownership. Top-level CRUD was org-scoped but children were not. A user in org A could access resources belonging to org B by constructing a direct URL to the child resource.

**Why it was missed:** Code review checked each endpoint individually. Each endpoint's own-level access control was correct. The gap was in the chain — no endpoint verified that its parent resource belonged to the caller's scope.

**What to look for:** Any nested resource endpoint. Verify it checks ownership up the full chain, not just the leaf. The multi-tenancy lens item "Nested resources: does the endpoint verify ownership up the full chain?" catches this.

### 2. Permission Name Used as Role Proxy

**What happened:** A frontend routing gate used `hasPermission("role:manage")` to show a platform-admin-only view. A lower-privilege role was later granted the same permission for a different, legitimate reason. The gate silently broke, exposing the platform-admin view to the lower-privilege role.

**Why it was missed:** No test verified which roles should NOT see the protected view. The permission matrix was reviewed in isolation from the frontend routing. The frontend gate was never traced back to the role-permission seed data.

**What to look for:** Any `hasPermission("X")` used as a visibility gate. Cross-reference which roles hold that permission. If more than one role has it, verify the gate is appropriate for all of them. The multi-tenancy lens item "Permission-as-role-proxy" catches this.

### 3. Data Access Bypass Through Indirection

**What happened:** Seven service-layer queries accessed the database through a multi-level chain (e.g., `self._repo._session.execute()`) that bypassed the scoped repository's own `execute()` method. The scoped layer enforced tenant filtering, but the indirect path skipped it entirely.

**Why it was missed:** Static analysis checked for direct session access (2-level pattern) but not 3-level chains. Code review didn't trace the full call path because each level looked correct in isolation.

**What to look for:** Any database access outside the designated data access layer. The multi-tenancy lens item "Data access isolation: are all queries routed through the scoped data access layer?" catches this. In reviews, trace the actual call path, not just the immediate method.

### 4. Silent Fail-Open on Missing Compliance Service

**What happened:** Services were constructed with optional dependencies (e.g., `compliance_repo: Type | None = None`). Code used `if repo is None: return` instead of raising. When factory methods omitted injection, compliance checks were silently skipped in production.

**Why it was missed:** The service worked correctly in tests where all dependencies were provided. The factory method that omitted the dependency was in a different module, and the service's null-check looked like defensive programming rather than a bug.

**What to look for:** Any security or compliance dependency typed as optional/nullable. The multi-tenancy lens item "Optional security dependencies" and "Silent skip patterns" catches this. Prefer non-optional types so missing injection is a type error, not a runtime skip.

### 5. Middleware Ordering Bug

**What happened:** Middleware was added in the wrong order due to the framework's counterintuitive reverse-wrapping semantics. A rate limiter tried to read user context before the authentication middleware had set it. Compounded by unverified base64 token decoding that enabled forged request floods.

**Why it was missed:** The middleware was added in what appeared to be the correct order based on the source code. The framework's actual execution order was the reverse of the declaration order. No integration test verified the execution sequence.

**What to look for:** Middleware/pipeline ordering. The security lens item "Middleware/pipeline execution order covered by an integration test?" catches this. Document the expected execution order in CLAUDE.md and verify it with a test.

### 6. Unrevocable Session Tokens

**What happened:** JWTs had no unique identifier (jti) and no revocation mechanism. Stolen tokens remained valid for the full TTL (4 hours) after the session was completed or the user's access was revoked.

**Why it was missed:** Token revocation was not in the original requirements. The JWT implementation followed standard tutorials that don't include revocation. The gap was visible only in the threat model, not in functional testing.

**What to look for:** Token lifecycle management. Do tokens have unique identifiers? Is there a revocation mechanism? What happens to active tokens when a session ends or access is revoked? This is a design-level concern, best caught during spec or plan review rather than code review.
