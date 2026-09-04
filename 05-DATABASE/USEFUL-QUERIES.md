# Useful Database Queries

Diagnostic, operational, and maintenance SQL queries for multi-tenant data platforms.

> [!WARNING]
> Always verify the active schema and confirm table filters before running mutating (`UPDATE`/`DELETE`) queries in production environments.

---

## 🔍 Diagnostic & Audit Queries

### 1. Count Total Entities and Storage Usage by Tenant
```sql
SELECT 
    t.tenant_id,
    t.slug,
    t.name,
    COUNT(e.id) AS total_entities,
    MAX(e.updated_at) AS latest_activity
FROM app_master.tenants t
LEFT JOIN app_master.entities e ON t.tenant_id = e.tenant_id
GROUP BY t.tenant_id, t.slug, t.name
ORDER BY total_entities DESC;
```

### 2. Find Users Assigned to a Specific Role Across Tenants
```sql
SELECT 
    u.id AS user_id,
    u.email,
    u.tenant_id,
    r.role_name,
    ur.assigned_at
FROM app_master.users u
JOIN app_master.user_roles ur ON u.id = ur.user_id
JOIN app_master.roles r ON ur.role_id = r.id
WHERE r.role_name = 'DATA_STEWARD'
ORDER BY u.tenant_id, u.email;
```

### 3. Identify Orphaned Records (Records with Invalid Tenant ID)
```sql
SELECT e.id, e.name, e.tenant_id
FROM app_master.entities e
LEFT JOIN app_master.tenants t ON e.tenant_id = t.tenant_id
WHERE t.tenant_id IS NULL;
```

---

## ⚙️ Maintenance & Index Checks

### Check Index Bloat & Missing Indexes for Tenant Filtering
```sql
SELECT 
    relname AS table_name,
    seq_scan,
    idx_scan,
    n_live_tup AS live_rows
FROM pg_stat_user_tables
WHERE schemaname = 'app_master'
ORDER BY seq_scan DESC
LIMIT 20;
```

---

## 🏷️ Tags
#database #sql #postgresql #queries #diagnostics
