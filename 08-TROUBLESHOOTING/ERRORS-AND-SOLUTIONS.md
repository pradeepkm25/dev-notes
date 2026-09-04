# Errors and Solutions Knowledge Base

Technical error logs, root cause analyses, investigation steps, and permanent fixes.

---

# ISSUE-001 — 403 Forbidden on Tenant Batch Ingest Endpoint

**Date:** 2026-09-04  
**Module:** Ingestion Service / IAM  

## Error
```json
{
  "timestamp": "2026-09-04T08:15:22Z",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied: Required authority [data:edit:all] is missing for user [steward@example.com]",
  "path": "/api/v1/tenants/tnt_abc123/entities/batch/upload"
}
```

## Symptoms
Data Steward users receive `403 Forbidden` whenever attempting to run the batch CSV catalog ingestion endpoint, even though they possess catalog creation rights in the UI.

## Root Cause
The batch ingestion service performs an upsert merge across all existing records (including records authored by other users). The controller guard enforces `@PreAuthorize("hasAuthority('data:edit:all')")`, which had not been mapped to the `DATA_STEWARD` role.

## Investigation Steps
1. Decoded user JWT token to inspect granted scopes and client roles.
2. Verified that user possessed `data:create` and `data:edit:own`, but lacked `data:edit:all`.
3. Inspected controller authorization annotation on `/batch/upload`.

## Solution
Added `data:edit:all` to the `DATA_STEWARD` role permission mapping table in the database and synchronized Keycloak client scopes:
```sql
INSERT INTO app_master.role_permissions (role_id, permission_id)
SELECT r.id, p.id 
FROM app_master.roles r, app_master.permissions p
WHERE r.role_name = 'DATA_STEWARD' AND p.permission_code = 'data:edit:all'
ON CONFLICT DO NOTHING;
```

## Prevention
Updated `03-ROLES-AND-PERMISSIONS/PERMISSION-CHECKLIST.md` and added an integration test verifying that `DATA_STEWARD` can execute batch updates.

## Related APIs / Tables / Configuration
- **APIs**: `POST /api/v1/tenants/{tenantId}/entities/batch/upload`
- **Tables**: `app_master.role_permissions`, `app_master.roles`
- **Configuration**: Keycloak Client Scope `tenant-backend-api-scopes`

## Tags
#troubleshooting #permissions #data-steward #403-forbidden #batch-ingest

---

# ISSUE-002 — Foreign Key Violation on User Deletion

**Date:** 2026-09-04  
**Module:** User Management / Database  

## Error
```text
org.postgresql.util.PSQLException: ERROR: update or delete on table "users" violates foreign key constraint "fk_audit_logs_user_id" on table "audit_logs"
  Detail: Key (id)=(usr_5541) is still referenced from table "audit_logs".
```

## Symptoms
Admin receives a 500 Internal Server Error when clicking "Delete User" on the admin console.

## Root Cause
The database foreign key `fk_audit_logs_user_id` does not have `ON DELETE SET NULL` or cascade deletion configured. Audit logs retain immutable historical records, preventing hard deletion of user primary keys.

## Investigation Steps
1. Inspected PostgreSQL schema DDL for `audit_logs` table.
2. Checked application delete handler and found it was performing a raw `userRepository.deleteById(userId)`.

## Solution
Switched user removal from hard-delete to soft-deletion (`status = 'DEACTIVATED'`, `deactivated_at = NOW()`):
```sql
UPDATE app_master.users 
SET status = 'DEACTIVATED', updated_at = NOW() 
WHERE id = 'usr_5541' AND tenant_id = 'tnt_abc123';
```

## Prevention
Enforced soft-delete pattern across all user management service APIs and added a safety check in `05-DATABASE/DELETE-FLOWS.md`.

## Related APIs / Tables / Configuration
- **Tables**: `app_master.users`, `app_master.audit_logs`
- **APIs**: `DELETE /api/v1/tenants/{id}/users/{userId}`

## Tags
#troubleshooting #database #postgresql #foreign-keys #soft-delete
