# Governance Service

**Module:** Governance  
**Repository:** `akashic-governance-api`  

---

## 🎯 Scope & Responsibilities
- Policy management, compliance audits, catalog curation, and access governance.
- Data Steward role permissions (`CREATE` and `EDIT_ALL` permissions for catalog and entity curation).
- Bot token issuance (`/governance/bot-token`) and system service account authorization.
- Inter-service user synchronization and governance role mapping.

---

## 📚 Notes in this Folder
- [`data-steward-create-edit-all-permissions.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/GOVERNANCE/data-steward-create-edit-all-permissions.md) — Mandatory `CREATE` and `EDIT_ALL` permission requirements for Data Stewards in Governance.

---

## 🔑 Keycloak Integration
- **Client ID**: `{tenantName}-governance`
- **Client Type**: Confidential (Standard flow + Direct Access Grants).
- **Callback URIs**: `{governanceUrl}/api/v1/auth/callback`, `{governanceUrl}/callback`.

---

## ⚡ Common Operations & Gotchas
- **Data Steward Permissions**: Governance requires both `CREATE` and `EDIT_ALL` permissions mapped to Data Stewards for dataset publishing and catalog updates.
- **Bot Token Validation**: Bot tokens use dedicated client scopes; always verify expiry and tenant scoping before calling downstream APIs.
- **Role Sync**: Governance roles require synchronization with Keycloak client roles on user onboarding.

---

## 🏷️ Tags
#governance #bot-token #policies #compliance #data-steward #permissions #keycloak
