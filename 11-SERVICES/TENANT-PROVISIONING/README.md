# Tenant Provisioning Service

**Module:** Tenant Provisioning  
**Repository:** `akashic-tenant-provisioning-api`  

---

## 🎯 Scope & Responsibilities
- Automated orchestration of new tenant environments:
  1. Keycloak realm creation (`akashic-{tenantName}-realm`).
  2. User profile schema updates and custom attributes.
  3. Client provisioning (`akashic-{tenantName}`, `akashic-{tenantName}-admin-service`, `akashic-system`, `akashic-bi`, `{tenantName}-governance`).
  4. Platform database configuration persistence and runtime cache refresh.

---

## 🔑 Key Service Clients & Protocol Mappers
- **Public Client**: `akashic-{tenantName}` (User attribute mapper: `tenant_name`).
- **Admin Service Client**: `akashic-{tenantName}-admin-service` (Service account + `realm-management` roles).
- **System Client**: `akashic-system` (Service account; requires Hardcoded Claim `tenant_name`, see [`system-client-tenant-name-claim.md`](file:///d:/Dhira-Work/dev-notes/06-KEYCLOAK/system-client-tenant-name-claim.md)).
- **BI Client**: `akashic-bi` (Confidential client).
- **Governance Client**: `{tenantName}-governance` (Confidential client).

---

## ⚡ Common Operations & Gotchas
- **Service Account Token Sanitization**: Realm creation automatically purges `Client Host` and `Client IP Address` mappers from the `service_account` client scope to keep M2M tokens clean and secure (see [`remove-client-host-address-token-mappers.md`](file:///d:/Dhira-Work/dev-notes/06-KEYCLOAK/remove-client-host-address-token-mappers.md)).
- **Brute Force Detection (Upcoming Task)**: Future enhancement `ENH-006` to enable automated temporary lockout on failed logins during realm creation (see [`brute-force-detection-configuration.md`](file:///d:/Dhira-Work/dev-notes/06-KEYCLOAK/brute-force-detection-configuration.md)).
- **Idempotency**: All client and realm creation helper methods check `kcAdminClient.clients.find(...)` first to allow safe re-runs on failed provisioning attempts.
- **Runtime Refresh**: Step 14 invokes `tenantRefreshService.refreshTenantRuntime(tenantName)` to notify live microservices of newly provisioned tenants.

---

## 🏷️ Tags
#tenant-provisioning #keycloak #automation #onboarding #clients
