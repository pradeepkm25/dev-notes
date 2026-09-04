# Tenant Admin Service

**Module:** Tenant Administration  
**Repository:** `akashic-tenant-admin-service-api`  

---

## 🎯 Scope & Responsibilities
- Tenant settings management, dynamic feature flag controls, and storage quota allocations.
- Tenant user onboarding, role assignments, password resets, and user deactivation.

---

## 🔑 Keycloak Integration
- **Client ID**: `akashic-{tenantName}-admin-service`
- **Role Permissions**: Possesses `realm-management` admin roles to manage tenant realm users.
- **Protocol Mappers**: `TENANT_NAME_MAPPER` (`tenant_name` user attribute mapper).

---

## ⚡ Common Operations & Gotchas
- **Cross-Tenant Guard**: Verify caller's JWT `tenant_id` matches the target tenant record before mutating any tenant settings or roles.
- **Soft Deletion**: Never hard-delete tenant users; update `status = 'DEACTIVATED'` to preserve historical audit logs.

---

## 🏷️ Tags
#tenant-admin #tenant-settings #user-management #feature-flags #keycloak
