# Role & Permission Evaluation Flows

Detailed sequence flows illustrating how permissions are resolved, embedded into JWT tokens, and enforced at runtime.

---

## 🔄 Permission Resolution Sequence

```text
[ User Login ]              [ Keycloak IAM ]               [ API Gateway / Microservice ]
      │                            │                                      │
      │── 1. Authenticate ────────>│                                      │
      │                            │── Load User Roles (e.g. DATA_STEWARD)│
      │                            │── Map Role Permissions to Scope      │
      │<── 2. Issue JWT ───────────│   (claims: roles, permissions)       │
      │    (Signed Token)          │                                      │
      │                                                                   │
      │── 3. Mutating Request (e.g., PUT /api/v1/entities/{id}) ─────────>│
      │      (Header: Authorization: Bearer <JWT>)                        │── 4. Verify Signature & aud
      │                                                                   │── 5. Evaluate Method Guard:
      │                                                                   │      @PreAuthorize("hasAuthority('data:edit:all')")
      │                                                                   │── 6. Check Tenant ID Context
      │<── 7. Return 200 OK (or 403 Forbidden if check fails) ────────────│
```

---

## 💡 Best Practice: Granular Permission Bits vs Role Checks

- **Do Not Enforce Roles in Controllers**: Avoid checks like `hasRole('TENANT_ADMIN')` in business logic controllers because roles evolve and tenants often request custom permission combinations.
- **Enforce Permissions**: Always guard endpoints with granular permissions like `hasAuthority('data:edit:all')`. This allows changing which roles possess specific permissions without changing backend code.

---

## 🏷️ Tags
#permissions #roles #authorization #security #architecture #flows
