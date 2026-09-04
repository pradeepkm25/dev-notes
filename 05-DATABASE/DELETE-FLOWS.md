# Safe Deletion Flows & Cleanup Protocols

Standard operating procedures for soft-deletions, hard purges, and tenant offboarding to prevent data corruption and foreign key constraint violations.

---

## ⚠️ Golden Rules of Deletion

> [!CAUTION]
> 1. Never delete directly from the `tenants` or `users` table without verifying dependent entities.
> 2. Prefer **soft deletes** (`is_deleted = true` or `deleted_at = NOW()`) for user-facing actions.
> 3. Perform hard purges inside an explicit transaction with row count validations.

---

## 🗑️ Safe Entity Deletion Flow

### Step 1: Pre-Deletion Count Verification
```sql
-- Check total records that will be affected
SELECT COUNT(*) 
FROM app_master.entity_attributes 
WHERE entity_id = 'ent_99812' AND tenant_id = 'tnt_abc123';
```

### Step 2: Transactional Cascade Deletion
```sql
BEGIN;

-- 1. Delete dependent child attributes
DELETE FROM app_master.entity_attributes
WHERE entity_id = 'ent_99812' AND tenant_id = 'tnt_abc123';

-- 2. Delete audit mappings / log references
DELETE FROM app_master.entity_tags
WHERE entity_id = 'ent_99812' AND tenant_id = 'tnt_abc123';

-- 3. Delete parent entity
DELETE FROM app_master.entities
WHERE id = 'ent_99812' AND tenant_id = 'tnt_abc123';

-- Inspect before committing:
-- ROLLBACK;  -- if row count was unexpected
COMMIT;
```

---

## 🏢 Complete Tenant Offboarding / Purge Protocol

When offboarding a tenant permanently (e.g., end of trial or GDPR right-to-be-forgotten):

1. **Step 1: Soft Suspend**: Update `tenants.status = 'SUSPENDED'` to immediately cut off API & login access.
2. **Step 2: Archive S3 Storage**: Transfer `/tenants/{tenant_id}/` to Glacier or generate a final backup archive.
3. **Step 3: Delete Keycloak Realm / Client**: Remove tenant users and client mapping in Keycloak.
4. **Step 4: Purge Database Records in Order**:
   - `audit_logs` (where `tenant_id = :id`)
   - `entity_attributes`
   - `entities`
   - `user_roles`
   - `users`
   - `tenant_settings`
   - `tenants`

---

## 🏷️ Tags
#database #deletion #cleanup #gdpr #transactions #cascade
