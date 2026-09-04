# Important Development Reminders

High-value rules, essential practices, and non-negotiable guidelines across tenant management, authentication, security, database operations, and APIs.

---

## 🏢 Tenant Setup

- **Verify All Default Role Permissions**: Always confirm that newly provisioned tenants have the foundational roles (`TENANT_ADMIN`, `DATA_STEWARD`, `MEMBER`, `VIEWER`) properly assigned with correct permission mappings.
- **Test Configuration with an Actual User**: Never conclude tenant provisioning verification using only the Super Admin account. Always log in with a newly minted standard tenant user to verify isolation and routing.
- **Enforce Tenant Isolation in Queries**: Always append `tenant_id = :tenantId` to database queries or verify that your ORM tenant filter / Hibernate filter is actively enabled for the current request context.
- **Isolate Tenant Cloud Storage**: Use `/tenants/{tenant_id}/` as the top-level path prefix for S3/Blob storage to ensure clean tenant offboarding and lifecycle management.

---

## 🛡️ Roles and Permissions

- **Document Required Permissions for Each Role**: Keep `03-ROLES-AND-PERMISSIONS/PERMISSIONS.md` updated whenever new API endpoints or capabilities are introduced.
- **Check Both Frontend and Backend Permission Requirements**:
  - Frontend controls UI button rendering and page route protection.
  - Backend controllers MUST strictly enforce security annotations (`@PreAuthorize("hasAuthority('...')")`). Never rely solely on frontend hiding.
- **No Hardcoded Role Strings**: Reference roles and permissions through typed constants or enums instead of raw strings to avoid casing bugs (`tenant_admin` vs `TENANT_ADMIN`).

---

## 🌐 APIs

- **Remove Sensitive Tokens Before Committing Curl Commands**: Strip `Authorization: Bearer <token>` and replace with `YOUR_ACCESS_TOKEN` in documentation and bug reports.
- **Ensure Idempotency on Mutating Endpoints**: Provide idempotency keys or unique constraint handling on POST/PUT operations to prevent duplicate creation on network retries.
- **Enforce Consistent Error Payloads**: Always return standardized JSON error bodies containing `errorCode`, `message`, `timestamp`, and `details`.

---

## 🗄️ Database

- **Check Foreign Key Dependencies Before Deleting Records**: Verify child records and cascade dependencies prior to manual data cleanup or tenant user purge operations.
- **Do Not Execute Destructive Queries Without Verification**:
  - Always run `SELECT COUNT(*)` with the exact same `WHERE` clause before executing a `DELETE` or `UPDATE` statement.
  - Always wrap manual database maintenance in explicit transactions: `BEGIN; ... ROLLBACK;` or `COMMIT;`.
- **Verify Schema Search Paths**: In schema-per-tenant PostgreSQL configurations, confirm `search_path` is explicitly set before running DDL or migrations.

---

## 🔑 Keycloak

- **Validate Token Audience and Client Scopes**: Check that generated JWT tokens contain the correct `aud` (audience) and `resource_access` client roles for the target backend service.
- **Explicit Redirect URIs**: Avoid wildcard `*` redirect URIs in staging/production environments; use explicit domain URLs.
- **Handle Silent Token Refresh**: Ensure frontend client SDKs handle short-lived access token renewal (5–15 mins) using refresh tokens without forcing user logout.

---

## 🔒 Security

- **Never Commit Secrets**: Zero access tokens, passwords, private keys, client secrets, or production URLs in git.
- **Use Standard Placeholders**: Always use `YOUR_ACCESS_TOKEN`, `YOUR_CLIENT_SECRET`, `YOUR_TENANT_ID`, and `api.example.com`.
- **Sanitize Error Responses**: Never leak internal database stack traces, SQL syntax errors, or server hostnames in client-facing API responses.

---

## 🚀 Before Closing a Feature

- **Run the Feature Completion Checklist**: Review [`09-CHECKLISTS/FEATURE-COMPLETION-CHECKLIST.md`](file:///d:/Dhira-Work/dev-notes/09-CHECKLISTS/FEATURE-COMPLETION-CHECKLIST.md) before marking tickets as done or submitting pull requests.
- **Log Reusable Discoveries**: Capture any reusable enhancement in [`07-ENHANCEMENTS/REUSABLE-ENHANCEMENTS.md`](file:///d:/Dhira-Work/dev-notes/07-ENHANCEMENTS/REUSABLE-ENHANCEMENTS.md) or [`07-ENHANCEMENTS/FUTURE-IMPLEMENTATION.md`](file:///d:/Dhira-Work/dev-notes/07-ENHANCEMENTS/FUTURE-IMPLEMENTATION.md).
