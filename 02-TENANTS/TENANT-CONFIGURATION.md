# Tenant Configuration Reference

Catalog of tenant configuration properties, feature flags, and environment variables required for standard and customized tenant deployments.

---

## 🔧 Core Configuration Parameters

| Config Key | Data Type | Default Value | Description |
| :--- | :--- | :--- | :--- |
| `TENANT_CODE` | String | *Required* | Unique alphanumeric slug (e.g., `acme-corp`). |
| `IS_ACTIVE` | Boolean | `true` | Master kill-switch to enable/disable tenant access. |
| `DEFAULT_LOCALE` | String | `en_US` | Default language and region code. |
| `TIMEZONE` | String | `UTC` | Default timezone for reports and scheduling. |
| `STORAGE_BUCKET_PREFIX` | String | `tenants/{tenant_id}/` | Base path in S3/cloud storage for file isolation. |
| `MAX_USER_LIMIT` | Integer | `50` | Maximum active user accounts permitted. |
| `STORAGE_QUOTA_GB` | Integer | `20` | Max storage capacity allocated for the tenant. |

---

## 🚩 Feature Flags

| Feature Flag Key | Default | Description | Impacted Modules |
| :--- | :--- | :--- | :--- |
| `FEATURE_SSO_ENABLED` | `false` | Enables SAML / OIDC enterprise SSO integrations. | Authentication / Keycloak |
| `FEATURE_AUDIT_EXPORT` | `true` | Allows Tenant Admin to export system audit logs to CSV. | Compliance / Reports |
| `FEATURE_BATCH_INGESTION` | `false` | Enables high-volume background data import endpoints. | Data Management / Jobs |
| `FEATURE_ADVANCED_PERMISSIONS`| `false` | Enables custom role builder beyond default 5 roles. | IAM / Security |
| `FEATURE_EMAIL_NOTIFICATIONS` | `true` | Enables outbound transactional notification emails. | Notifications Service |

---

## 📦 Database Configuration Schema

Tenant configuration is stored in the `tenant_settings` table as key-value pairs or structured JSON:

```sql
CREATE TABLE IF NOT EXISTS app_master.tenant_settings (
    tenant_id VARCHAR(64) NOT NULL,
    config_key VARCHAR(128) NOT NULL,
    config_value TEXT NOT NULL,
    data_type VARCHAR(32) DEFAULT 'STRING',
    is_encrypted BOOLEAN DEFAULT FALSE,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_by VARCHAR(128),
    PRIMARY KEY (tenant_id, config_key)
);
```

---

## 📝 Example Configuration Insert

```sql
INSERT INTO app_master.tenant_settings (tenant_id, config_key, config_value, data_type, is_encrypted)
VALUES 
    ('tnt_abc123', 'STORAGE_BUCKET_PREFIX', 'tenants/tnt_abc123/', 'STRING', false),
    ('tnt_abc123', 'MAX_USER_LIMIT', '100', 'INTEGER', false),
    ('tnt_abc123', 'FEATURE_BATCH_INGESTION', 'true', 'BOOLEAN', false),
    ('tnt_abc123', 'FEATURE_SSO_ENABLED', 'true', 'BOOLEAN', false)
ON CONFLICT (tenant_id, config_key) 
DO UPDATE SET config_value = EXCLUDED.config_value, updated_at = NOW();
```

---

## 🏷️ Tags
#tenant #configuration #feature-flags #settings #database
