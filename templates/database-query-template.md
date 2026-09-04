# Query Title

## Purpose
[Describe what this query retrieves, updates, or audits]

## Database / Schema
[e.g., PostgreSQL / `app_master` schema]

## When to Use
[e.g., Run before tenant monthly billing sync or during migration audits]

## Query
```sql
-- Example query
SELECT 
    t.tenant_id,
    t.name,
    COUNT(u.id) AS user_count
FROM app_master.tenants t
LEFT JOIN app_master.users u ON t.tenant_id = u.tenant_id
WHERE t.status = 'ACTIVE'
GROUP BY t.tenant_id, t.name;
```

## Explanation
[Explain the joins, filters, performance considerations]

## Related Tables
- `app_master.tenants`
- `app_master.users`

## Safety Warning
⚠️ Explain whether the query modifies or deletes data. If modifying, specify expected duration or lock scope.

## Verification Query
Provide a safe `SELECT` query that should be run before destructive operations:
```sql
SELECT COUNT(*) 
FROM app_master.users 
WHERE status = 'PENDING_DELETION' AND tenant_id = 'YOUR_TENANT_ID';
```

## Tags
#database #sql #postgresql #module-name
