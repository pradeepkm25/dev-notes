# Multi-Tenant Technical Flows

Technical workflows and lifecycle sequences for tenant provisioning, context extraction, and domain routing.

---

## 🔄 Flow 1: Tenant Provisioning Lifecycle

```text
[ Super Admin Console ]       [ Provisioning API ]        [ Keycloak IAM ]        [ DB (PostgreSQL) ]        [ S3 Storage ]
           │                           │                          │                        │                       │
           │── 1. POST /api/tenants ──>│                          │                        │                       │
           │      (slug, name, admin)  │── 2. Create Realm/Client>│                        │                       │
           │                           │                          │                        │                       │
           │                           │── 3. Insert Master Row ──────────────────────────>│                       │
           │                           │── 4. Apply Schema/Seed ──────────────────────────>│                       │
           │                           │                                                   │                       │
           │                           │── 5. Create Bucket Prefix ────────────────────────────────────────────────>│
           │                           │      (/tenants/{id}/)                             │                       │
           │                           │                                                   │                       │
           │                           │── 6. Create Initial Admin User ──>│               │                       │
           │                           │      (Send invite email)          │               │                       │
           │<── 7. Return 201 Created ─│                                                   │                       │
```

---

## 🔄 Flow 2: Runtime Request Tenant Context Extraction

Every incoming HTTP request resolves tenant context via a shared filter pipeline:

1. **Host Header or Subdomain**: Inspect incoming host (e.g., `acme-corp.app.example.com` or `custom-domain.com`).
2. **JWT Claim Extraction**: Extract and validate `tenant_id` from decoded JWT claims (`claims.get("tenant_id")`).
3. **Cross-Validation**: Verify that token `tenant_id` matches the resolved tenant domain.
4. **Context Injection**: Populate `TenantContextHolder` (ThreadLocal in Java or AsyncLocalStorage in Node.js) for downstream database filters and query builders.
5. **Context Cleanup**: In `finally` block, always call `TenantContextHolder.clear()` to prevent memory leaks across pooled threads.

---

## 🏷️ Tags
#tenant #flows #provisioning #architecture #context-resolution
