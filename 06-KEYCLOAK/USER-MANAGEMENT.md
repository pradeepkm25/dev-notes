# Keycloak User Management & Provisioning

Procedures and API patterns for creating, configuring, and managing users in Keycloak within a multi-tenant system.

---

## 👤 User Attribute Conventions

Every user in Keycloak must have standard custom attributes populated under **Attributes**:

| Attribute Key | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `tenant_id` | String | Unique tenant ID string | `tnt_abc123` |
| `tenant_slug` | String | URL-safe tenant identifier | `acme-corp` |
| `locale` | String | User preferred language | `en_US` |

---

## 🔑 Create Tenant User via Keycloak Admin REST API

```bash
curl -X POST "https://auth.example.com/admin/realms/YOUR_REALM/users" \
  -H "Authorization: Bearer YOUR_ADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "user@acme-corp.com",
    "email": "user@acme-corp.com",
    "firstName": "Jane",
    "lastName": "Doe",
    "enabled": true,
    "emailVerified": false,
    "attributes": {
      "tenant_id": ["tnt_abc123"],
      "tenant_slug": ["acme-corp"]
    },
    "requiredActions": ["UPDATE_PASSWORD", "VERIFY_EMAIL"]
  }'
```

---

## ✉️ Trigger Execute Actions Email (Password Setup / Activation)

```bash
curl -X PUT "https://auth.example.com/admin/realms/YOUR_REALM/users/USER_UUID/execute-actions-email" \
  -H "Authorization: Bearer YOUR_ADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '["UPDATE_PASSWORD", "VERIFY_EMAIL"]'
```

---

## 🏷️ Tags
#keycloak #user-management #provisioning #admin-api #onboarding
