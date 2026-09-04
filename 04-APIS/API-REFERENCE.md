# Core API Reference

Catalog of standard REST endpoints for tenant operations, catalog management, and administrative services.

> [!IMPORTANT]
> All endpoints require an Authorization header: `Authorization: Bearer YOUR_ACCESS_TOKEN`.
> Replace all placeholders (`YOUR_TENANT_ID`, `YOUR_ACCESS_TOKEN`, `api.example.com`) with actual values.

---

## 🏢 Tenant Management

### 1. Get Tenant Details
- **Method**: `GET`
- **Endpoint**: `/api/v1/tenants/{tenantId}`
- **Permissions**: `tenant:read`
- **Response**: `200 OK`
```json
{
  "tenantId": "tnt_abc123",
  "slug": "acme-corp",
  "name": "Acme Corporation",
  "status": "ACTIVE",
  "settings": {
    "STORAGE_BUCKET_PREFIX": "tenants/tnt_abc123/",
    "MAX_USER_LIMIT": 100
  },
  "createdAt": "2026-09-01T10:00:00Z"
}
```

### 2. Update Tenant Settings
- **Method**: `PUT`
- **Endpoint**: `/api/v1/tenants/{tenantId}/settings`
- **Permissions**: `tenant:update`
- **Request Body**:
```json
{
  "settings": {
    "FEATURE_BATCH_INGESTION": "true",
    "TIMEZONE": "America/New_York"
  }
}
```
- **Response**: `200 OK`

---

## 📦 Data & Catalog Management

### 1. Query Catalog Entities (Paged)
- **Method**: `GET`
- **Endpoint**: `/api/v1/tenants/{tenantId}/entities?page=0&size=20&sort=createdAt,desc`
- **Permissions**: `data:read`
- **Response**: `200 OK`
```json
{
  "content": [
    {
      "id": "ent_99812",
      "name": "Primary Product Catalog",
      "category": "INVENTORY",
      "status": "PUBLISHED",
      "createdBy": "user_456",
      "updatedAt": "2026-09-04T08:30:00Z"
    }
  ],
  "totalElements": 1,
  "totalPages": 1,
  "page": 0,
  "size": 20
}
```

### 2. Create Entity
- **Method**: `POST`
- **Endpoint**: `/api/v1/tenants/{tenantId}/entities`
- **Permissions**: `data:create`
- **Request Body**:
```json
{
  "name": "Primary Product Catalog",
  "category": "INVENTORY",
  "metadata": {
    "skuPrefix": "ACM",
    "version": "1.0"
  }
}
```
- **Response**: `201 Created`

---

## 🏷️ Tags
#api #reference #rest #endpoints #crud
