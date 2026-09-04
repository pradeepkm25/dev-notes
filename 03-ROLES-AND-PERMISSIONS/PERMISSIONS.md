# Granular Permissions Reference

Master reference of granular permission codes, descriptions, and their mapping between backend authorization guards and frontend UI controls.

---

## 🛡️ Permission Code Index

### Tenant Administration (`tenant:*`)
- `tenant:read`: View tenant profile and basic metadata.
- `tenant:update`: Modify tenant branding, settings, and flags.
- `tenant:delete`: Trigger tenant deactivation or scheduled soft deletion.
- `tenant:user:invite`: Send email invitations to new tenant users.
- `tenant:user:manage`: Change roles, lock, or deactivate existing tenant users.

### Data & Catalog Operations (`data:*`)
- `data:read`: Read-only access to published data and catalog assets.
- `data:create`: Permission to author new records or ingest items.
- `data:edit:own`: Permission to update records created by the current user.
- `data:edit:all`: Permission to edit records created by ANY user within the tenant.
- `data:delete:own`: Permission to delete records created by current user.
- `data:delete:all`: Permission to delete any record within the tenant.
- `data:import:batch`: Bulk ingestion API access (CSV/JSON streaming).
- `data:export:all`: Full tenant data export permission.

### Audit & Security (`audit:*`)
- `audit:logs:view`: Read tenant access and security audit logs.
- `audit:logs:export`: Download audit logs as CSV/JSON.

---

## 🔗 Role to Permission Mapping

```text
TENANT_ADMIN
 ├── tenant:* (all)
 ├── data:* (all)
 └── audit:* (all)

DATA_STEWARD
 ├── data:read
 ├── data:create
 ├── data:edit:own
 ├── data:edit:all          <-- Critical for steward curation
 ├── data:import:batch
 ├── data:export:all
 └── tenant:read

MEMBER
 ├── data:read
 ├── data:create
 ├── data:edit:own
 ├── data:delete:own
 └── tenant:read

VIEWER
 ├── data:read
 └── tenant:read
```

---

## 💻 Code Guard Reference

### Backend Guard (Spring Security / Node Express)
```java
// Spring Security PreAuthorize Example
@PreAuthorize("hasAuthority('data:edit:all') or (hasAuthority('data:edit:own') and #entity.createdBy == authentication.name)")
@PutMapping("/api/v1/entities/{id}")
public ResponseEntity<EntityDTO> updateEntity(@PathVariable String id, @RequestBody EntityDTO dto) { ... }
```

### Frontend Guard (React / Vue)
```tsx
// React Can Access Component Example
<Can I="data:import:batch">
  <BatchImportButton onClick={handleBatchImportModal} />
</Can>
```

---

## 🏷️ Tags
#permissions #rbac #abac #authorization #guards
