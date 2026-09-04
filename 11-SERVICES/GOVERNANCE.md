# Governance Service

**Module:** Governance  
**Repository:** `akashic-governance-api` (or related governance module)  

---

## 🎯 Scope & Responsibilities
- Policy management, compliance audits, and data access governance.
- Bot token issuance (`/governance/bot-token`) and system service account authorization.
- Inter-service user synchronization and governance role mapping.

---

## 🔑 Keycloak Integration
- **Client ID**: `{tenantName}-governance`
- **Client Type**: Confidential (Standard flow + Direct Access Grants).
- **Callback URIs**: `{governanceUrl}/api/v1/auth/callback`, `{governanceUrl}/callback`.

---

## ⚡ Common Operations & Gotchas
- **Bot Token Validation**: Bot tokens use dedicated client scopes; always verify expiry and tenant scoping before calling downstream APIs.
- **Role Sync**: Governance roles require synchronization with Keycloak client roles on user onboarding.

---

## 🏷️ Tags
#governance #bot-token #policies #compliance #keycloak
