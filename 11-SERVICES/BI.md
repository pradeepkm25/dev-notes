# Business Intelligence (BI) Service

**Module:** BI / Analytics  
**Client ID:** `akashic-bi`  

---

## 🎯 Scope & Responsibilities
- Embedded dashboard analytics, metrics visualization, and semantic query execution.
- Tenant-isolated BI queries and report exports.

---

## 🔑 Keycloak Integration
- **Client ID**: `akashic-bi`
- **Client Type**: Confidential (Service accounts enabled, standard flow enabled).
- **Protocol Mappers**: `TENANT_NAME_MAPPER` (`tenant_name` user attribute mapper).
- **Callback URIs**: `{akashicBiUrl}/*`, `{akashicBiUrl}/oauth-authorized/keycloak`.

---

## ⚡ Common Operations & Gotchas
- **Cross-Tenant Query Prevention**: Ensure SQL queries executed by BI engine enforce tenant filtering (`WHERE tenant_id = :tenantId`) or use schema-per-tenant isolation.
- **Session Timeout**: Keep BI iframe session timeout synchronized with Keycloak access token lifespan.

---

## 🏷️ Tags
#bi #analytics #dashboards #keycloak #reporting
