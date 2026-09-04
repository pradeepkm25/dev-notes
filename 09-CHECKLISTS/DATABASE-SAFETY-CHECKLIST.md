# Database Safety Checklist

Mandatory verification protocol to execute prior to running manual database updates, migrations, or data purges in any environment.

---

## 🛡️ Database Execution Protocol

```text
SELECT first
     ↓
Verify affected records
     ↓
Check foreign key dependencies
     ↓
Run DELETE / UPDATE only when confirmed
```

---

## 📋 Verification Checklist

- [ ] **Verify Environment**: Double-check connection string and database name (`DEV`, `STAGING`, `PROD`). Never run ad-hoc queries against production without formal approval.
- [ ] **Run SELECT First**: Execute a `SELECT COUNT(*)` with the EXACT `WHERE` clause intended for the update or delete operation.
- [ ] **Check Affected Rows**: Ensure the row count matches expectations before proceeding.
- [ ] **Check Foreign Keys**: Verify whether child tables reference the target IDs (`ON DELETE RESTRICT` will fail; un-indexed FKs can lock parent tables).
- [ ] **Backup if Necessary**: For large data migrations or destructive fixes, take a table snapshot or confirm the latest RDS/PostgreSQL automated backup is healthy.
- [ ] **Review DELETE / UPDATE Conditions**: Verify that no un-scoped `WHERE 1=1` or missing `tenant_id` clauses exist.
- [ ] **Execute Inside a Transaction**: Wrap statements inside `BEGIN; ... COMMIT;` so they can be rolled back immediately if row counts deviate.
- [ ] **Verify Result**: Run a post-execution `SELECT` to verify the state of the data.

---

## 🏷️ Tags
#checklist #database #sql #safety #postgresql #data-integrity
