# Development Inbox

> Add quick discoveries here immediately.
> Do not spend time making the note perfect.
> Organize it later.

---

## 2026-09-04

### Title
Data Steward Requires Dual Permissions for Catalog Batch Ingest

### Discovery
The `DATA_STEWARD` role fails with `403 Forbidden` on `/api/v1/tenants/{id}/entities/batch/upload` unless granted both `data:create` AND `data:edit:all`. This is because the batch pipeline updates existing shared catalog records during the upsert phase.

### Where It Can Be Useful
Tenant onboarding automation scripts, Keycloak client scope mapping, and role matrix tests for all new tenants.

### Possible Action
Update the default role provisioning SQL seed and add a validation check in the tenant onboarding test suite.

### Tags
#tenant #permissions #data-steward #keycloak #security

### Status
- [ ] Not reviewed

---

## 2026-09-04

### Title
Foreign Key Constraint Blocks Tenant User Deletion

### Discovery
Attempting to hard-delete a user record directly from `app_master.users` throws `PSQLException: update or delete on table "users" violates foreign key constraint "fk_audit_logs_user_id" on table "audit_logs"`. 
Child records in `audit_logs` must either be nulled out (`ON DELETE SET NULL`) or user deactivation should be handled strictly via soft-delete (`status = 'DEACTIVATED'`).

### Where It Can Be Useful
User management service, GDPR account purge workflows, and database migration cleanup scripts.

### Possible Action
Document in `05-DATABASE/DELETE-FLOWS.md` and enforce soft-deletion across all tenant user management endpoints.

### Tags
#database #sql #postgresql #foreign-keys #delete-flows #error

### Status
- [ ] Not reviewed

---

<!-- Add new discoveries above this line using templates/quick-note-template.md -->
