# Role Definitions & Hierarchy

Overview of system and tenant-level roles, inheritance hierarchies, and scope of authority.

---

## 👑 Role Hierarchy

```text
SUPER_ADMIN (Platform Level - Cross Tenant)
     │
     └── TENANT_ADMIN (Tenant Level Master)
              │
              ├── DATA_STEWARD (Catalog & Master Data Manager)
              │        │
              │        └── MEMBER (Standard User - CRUD own resources)
              │                 │
              │                 └── VIEWER (Read-Only User)
              │
              └── BILLING_ADMIN (Specialized Financial / Plan Access)
```

---

## 👥 Role Descriptions

### 1. `SUPER_ADMIN`
- **Scope**: Platform-wide (Cross-tenant).
- **Description**: Operations team and system maintainers. Can provision new tenants, alter system settings, and inspect system logs.
- **Key Capabilities**: Create tenants, suspend tenants, global schema migrations, view multi-tenant telemetry.

### 2. `TENANT_ADMIN`
- **Scope**: Single Tenant.
- **Description**: Primary administrator of a specific tenant organization.
- **Key Capabilities**: User onboarding, role assignment, tenant feature toggle configuration, audit log export, tenant-wide settings.

### 3. `DATA_STEWARD`
- **Scope**: Single Tenant.
- **Description**: Power user responsible for catalog maintenance, master data curation, and batch ingestion.
- **Key Capabilities**: Batch import, mass update of entities, category & taxonomy management, metadata definition.
- **Important Gotcha**: Requires both `DATA_CREATE` and `DATA_EDIT_ALL` permissions.

### 4. `MEMBER`
- **Scope**: Single Tenant.
- **Description**: Regular day-to-day operational user.
- **Key Capabilities**: Create and edit own records, collaborate on shared resources, upload documents.

### 5. `VIEWER`
- **Scope**: Single Tenant.
- **Description**: Read-only guest or auditor.
- **Key Capabilities**: View dashboards, generate reports, view public/published tenant catalogs.

---

## 📊 Role Matrix Summary

| Capability | SUPER_ADMIN | TENANT_ADMIN | DATA_STEWARD | MEMBER | VIEWER |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Provision / Delete Tenants | ✅ | ❌ | ❌ | ❌ | ❌ |
| Manage Tenant Users & Roles | ✅ | ✅ | ❌ | ❌ | ❌ |
| Configure Tenant Settings | ✅ | ✅ | ❌ | ❌ | ❌ |
| Batch Data Import / Export | ✅ | ✅ | ✅ | ❌ | ❌ |
| Edit Any Tenant Record | ✅ | ✅ | ✅ | ❌ | ❌ |
| Create / Edit Own Records | ✅ | ✅ | ✅ | ✅ | ❌ |
| Read / View Published Data | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## 🏷️ Tags
#roles #iam #security #permissions #matrix
