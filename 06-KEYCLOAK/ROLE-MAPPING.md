# Keycloak Role Mapping & Client Scope Architecture

Design principles for mapping Realm Roles, Client Roles, and Scopes to JWT claims for backend authorization.

---

## 🏛️ Realm Roles vs Client Roles

- **Realm Roles**: Global across the realm (e.g., `offline_access`, `uma_authorization`). Use sparingly for multi-tenant applications.
- **Client Roles**: Scoped to the specific frontend/backend client (e.g., client `tenant-backend-api` defines roles `TENANT_ADMIN`, `DATA_STEWARD`, `MEMBER`, `VIEWER`). **This is the recommended approach**.

---

## 🗺️ Role-to-Client Assignment API

### Assign Client Role to User
```bash
# 1. Fetch Client UUID
CLIENT_UUID=$(curl -s -X GET "https://auth.example.com/admin/realms/YOUR_REALM/clients?clientId=tenant-backend-api" \
  -H "Authorization: Bearer YOUR_ADMIN_ACCESS_TOKEN" | jq -r '.[0].id')

# 2. Fetch Role Representation
ROLE_REP=$(curl -s -X GET "https://auth.example.com/admin/realms/YOUR_REALM/clients/${CLIENT_UUID}/roles/DATA_STEWARD" \
  -H "Authorization: Bearer YOUR_ADMIN_ACCESS_TOKEN")

# 3. Map Role to User
curl -X POST "https://auth.example.com/admin/realms/YOUR_REALM/users/USER_UUID/role-mappings/clients/${CLIENT_UUID}" \
  -H "Authorization: Bearer YOUR_ADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "[${ROLE_REP}]"
```

---

## 🏷️ Tags
#keycloak #role-mapping #client-roles #jwt #iam
