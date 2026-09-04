# Development Knowledge Base Index

A centralized, master index of all reference documents, checklists, templates, and guides across the knowledge base.

---

## ⚡ Quick Access

- 📥 [Quick Capture Inbox](00-INBOX/INBOX.md)
- 📌 [Important Reminders](01-IMPORTANT-REMINDERS/IMPORTANT-REMINDERS.md)
- 💡 [Reusable Enhancements](07-ENHANCEMENTS/REUSABLE-ENHANCEMENTS.md)
- 🎯 [Future Implementation Tracker](07-ENHANCEMENTS/FUTURE-IMPLEMENTATION.md)
- 🤖 [Automation Opportunities](07-ENHANCEMENTS/AUTOMATION-OPPORTUNITIES.md)
- 🛠️ [Errors and Solutions](08-TROUBLESHOOTING/ERRORS-AND-SOLUTIONS.md)
- 🧠 [Lessons Learned](08-TROUBLESHOOTING/LESSONS-LEARNED.md)
- ✅ [Feature Completion Checklist](09-CHECKLISTS/FEATURE-COMPLETION-CHECKLIST.md)

---

## 📚 Topics Directory

### 🏢 02 - Tenants
- [Tenant Setup Checklist](02-TENANTS/TENANT-SETUP-CHECKLIST.md) — Step-by-step tenant provisioning pipeline.
- [Tenant Configuration](02-TENANTS/TENANT-CONFIGURATION.md) — Feature flags, configuration schema, and property catalog.
- [Tenant Technical Flows](02-TENANTS/TENANT-FLOWS.md) — Multi-tenant lifecycle and data segregation flows.
- [Tenant-Specific Notes](02-TENANTS/TENANT-SPECIFIC-NOTES.md) — Bespoke overrides, custom domains, and custom rate limits.

### 🛡️ 03 - Roles & Permissions
- [Roles Reference](03-ROLES-AND-PERMISSIONS/ROLES.md) — System and tenant-level hierarchy (`SUPER_ADMIN`, `TENANT_ADMIN`, `DATA_STEWARD`, `MEMBER`, `VIEWER`).
- [Permissions Catalog](03-ROLES-AND-PERMISSIONS/PERMISSIONS.md) — Granular permission definitions and backend/frontend code guards.
- [Permission Verification Checklist](03-ROLES-AND-PERMISSIONS/PERMISSION-CHECKLIST.md) — Security validation before merging features.
- [Role & Permission Flows](03-ROLES-AND-PERMISSIONS/ROLE-PERMISSION-FLOWS.md) — Token claim enrichment and runtime permission checks.

### 🌐 04 - APIs
- [API Reference](04-APIS/API-REFERENCE.md) — Standard REST endpoints, request/response models.
- [Curl Commands](04-APIS/CURL-COMMANDS.md) — Ready-to-use curl snippets with placeholder authorization.
- [API Flows](04-APIS/API-FLOWS.md) — Multi-step sequence interactions (Token exchange, Batch upload).
- [API Troubleshooting](04-APIS/API-TROUBLESHOOTING.md) — Diagnosing 401, 403, 429, and gateway timeouts.

### 🗄️ 05 - Database
- [Useful Queries](05-DATABASE/USEFUL-QUERIES.md) — Audit, diagnostic, and performance inspection queries.
- [Table Relationships](05-DATABASE/TABLE-RELATIONSHIPS.md) — ER diagrams and multi-tenant key conventions.
- [Delete Flows](05-DATABASE/DELETE-FLOWS.md) — Safe deletion cascades and tenant cleanup procedures.
- [Database Troubleshooting](05-DATABASE/DATABASE-TROUBLESHOOTING.md) — Lock resolution, connection pool exhaustion, schema migration errors.

### 🔑 06 - Keycloak
- [Keycloak Flows](06-KEYCLOAK/KEYCLOAK-FLOWS.md) — OIDC authentication, token mapping, and service accounts.
- [User Management](06-KEYCLOAK/USER-MANAGEMENT.md) — User onboarding, attribute mapping, and password reset flows.
- [Role Mapping](06-KEYCLOAK/ROLE-MAPPING.md) — Realm roles vs client roles and scope evaluators.
- [Common Issues](06-KEYCLOAK/COMMON-ISSUES.md) — Resolving `redirect_uri` mismatches, missing claims, CORS, and audience errors.

### 💡 07 - Enhancements
- [Reusable Enhancements](07-ENHANCEMENTS/REUSABLE-ENHANCEMENTS.md) — Architectural patterns (`ENH-001`, `ENH-002`, ...).
- [Future Implementation](07-ENHANCEMENTS/FUTURE-IMPLEMENTATION.md) — Master backlog table with priorities and statuses.
- [Automation Opportunities](07-ENHANCEMENTS/AUTOMATION-OPPORTUNITIES.md) — Repetitive tasks identified for automated scripting (`AUTO-001`, ...).
- [Implemented Enhancements](07-ENHANCEMENTS/IMPLEMENTED-ENHANCEMENTS.md) — Completed feature enhancements and retrospective learnings.

### 🛠️ 08 - Troubleshooting
- [Errors and Solutions](08-TROUBLESHOOTING/ERRORS-AND-SOLUTIONS.md) — Documented problem-solution logs (`ISSUE-001`, ...).
- [Lessons Learned](08-TROUBLESHOOTING/LESSONS-LEARNED.md) — Engineering retrospectives and takeaways (`LESSON-001`, ...).
- [Common Problems](08-TROUBLESHOOTING/COMMON-PROBLEMS.md) — Quick lookup for frequently encountered environment gotchas.

### ✅ 09 - Checklists
- [New Tenant Checklist](09-CHECKLISTS/NEW-TENANT-CHECKLIST.md) — Rollout and onboarding verification.
- [Feature Completion Checklist](09-CHECKLISTS/FEATURE-COMPLETION-CHECKLIST.md) — 10-point definition of done before PR closure.
- [API Testing Checklist](09-CHECKLISTS/API-TESTING-CHECKLIST.md) — Security, validation, edge case, and tenant isolation checks.
- [Database Safety Checklist](09-CHECKLISTS/DATABASE-SAFETY-CHECKLIST.md) — Pre-execution checklist for updates and deletes.

### 📦 10 - Archive
- [Archive Directory](10-ARCHIVE/README.md) — Repository of deprecated notes and obsolete patterns.

### 📝 Templates
- [Quick Note Template](templates/quick-note-template.md)
- [Enhancement Proposal Template](templates/enhancement-template.md)
- [Issue & Solution Template](templates/issue-solution-template.md)
- [API Documentation Template](templates/api-template.md)
- [Database Query Template](templates/database-query-template.md)
- [Technical Flow Template](templates/technical-flow-template.md)
- [Tenant Note Template](templates/tenant-note-template.md)
- [Lesson Learned Template](templates/lesson-learned-template.md)

---

## 📌 How to Update this Index

Whenever you create a new major reference file or modular topic guide:
1. Add a link and 1-sentence summary under the corresponding topic heading above.
2. Ensure file naming matches the convention (`UPPERCASE-FOR-MAIN-REFERENCE.md` or `lowercase-sub-topic.md`).
