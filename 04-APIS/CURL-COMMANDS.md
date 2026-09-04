# Reusable Curl Commands

Ready-to-use curl templates for manual API testing and automation scripts.

> [!CAUTION]
> Never commit real access tokens or client secrets. Use the provided placeholders (`YOUR_ACCESS_TOKEN`, `YOUR_CLIENT_SECRET`, `YOUR_TENANT_ID`).

---

## 🔑 Keycloak Token Retrieval

### 1. Resource Owner Password Flow (User Login)
```bash
curl -X POST "https://auth.example.com/realms/YOUR_REALM/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "username=admin@example.com" \
  -d "password=YOUR_PASSWORD" \
  -d "scope=openid profile email"
```

### 2. Client Credentials Flow (Service-to-Service M2M)
```bash
curl -X POST "https://auth.example.com/realms/YOUR_REALM/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=YOUR_M2M_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET"
```

---

## 🏢 Tenant API Invocations

### 1. Fetch Tenant Profile
```bash
curl -X GET "https://api.example.com/api/v1/tenants/YOUR_TENANT_ID" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Accept: application/json"
```

### 2. Update Tenant Feature Setting
```bash
curl -X PUT "https://api.example.com/api/v1/tenants/YOUR_TENANT_ID/settings" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "settings": {
      "FEATURE_BATCH_INGESTION": "true"
    }
  }'
```

### 3. Create Entity / Catalog Record
```bash
curl -X POST "https://api.example.com/api/v1/tenants/YOUR_TENANT_ID/entities" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Q3 Inventory Schema",
    "category": "INVENTORY",
    "metadata": {
      "version": "1.0.0"
    }
  }'
```

---

## 🏷️ Tags
#api #curl #keycloak #testing #snippets
