# Role & Permission Verification Checklist

Use this checklist whenever adding a new feature, creating an endpoint, or modifying role assignments.

---

## 🔍 Pre-Release Authorization Checklist

### 1. API Security
- [ ] **No Public Leakage**: Is the endpoint protected behind authentication middleware (`Bearer` token required)?
- [ ] **Tenant Isolation Guard**: Does the controller ensure `tenant_id` from the JWT matches the target resource?
- [ ] **Method-Level Security**: Is `@PreAuthorize` / permission middleware applied to all mutating methods (POST, PUT, PATCH, DELETE)?
- [ ] **Cross-Tenant Access Test**: Test that User A with `TENANT_ADMIN` on `tenant-1` cannot access resources on `tenant-2` (expect `403 Forbidden` or `404 Not Found`).

### 2. Frontend / UI Integrity
- [ ] **Visual Permission Checks**: Are action buttons (e.g., "Delete", "Bulk Import", "Edit Settings") hidden or disabled for unauthorized roles?
- [ ] **Direct URL Navigation**: If a `VIEWER` directly navigates to `/admin/settings` or `/import`, does the router redirect to 403 or home?
- [ ] **Graceful Failure**: If a backend 403 is returned, does the UI show a clear, friendly error toast instead of crashing?

### 3. Role Matrix Verification
- [ ] **Super Admin**: Tested system-level operations.
- [ ] **Tenant Admin**: Tested tenant configuration, user management, full CRUD.
- [ ] **Data Steward**: Verified curation & batch import capabilities.
- [ ] **Member**: Verified CRUD on own resources; confirmed inability to edit other members' resources unless permitted.
- [ ] **Viewer**: Verified complete read-only experience (no mutating buttons visible).

---

## 🏷️ Tags
#checklist #permissions #security #testing
