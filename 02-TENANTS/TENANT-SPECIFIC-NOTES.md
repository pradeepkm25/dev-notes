# Tenant-Specific Notes & Overrides

This document tracks custom handling, temporary workarounds, non-standard configurations, or bespoke business rules applied to individual tenants.

---

## 📑 Tenant Directory

- [Acme Corporation (`tnt_acme_corp`)](#acme-corporation-tnt_acme_corp)
- [Beta Logistics (`tnt_beta_logistics`)](#beta-logistics-tnt_beta_logistics)
- [Template for New Tenant Entry](#template-for-new-tenant-entry)

---

## Acme Corporation (`tnt_acme_corp`)

### Environment: Production / Staging
- **Tenant ID**: `tnt_acme_corp`
- **Slug**: `acme-corp`
- **Primary Admin**: `admin@acme-corp.com`

### Custom Overrides & Notes
1. **Custom SSO Provider**:
   - Uses dedicated Azure AD OIDC broker in Keycloak.
   - Redirect URI pattern: `https://acme-corp.example.com/oauth2/callback`.
2. **High-Volume Batch Limits**:
   - Batch upload rate limit raised from `10 req/min` to `50 req/min` via `RATE_LIMIT_BATCH_INGEST` setting.
3. **Data Retention Policy**:
   - 90-day automated purge policy on audit logs (enforced via DB scheduled job).

---

## Beta Logistics (`tnt_beta_logistics`)

### Environment: Staging
- **Tenant ID**: `tnt_beta_logistics`
- **Slug**: `beta-logistics`
- **Primary Admin**: `dev-lead@betalogistics.example`

### Custom Overrides & Notes
1. **Disabled Module**:
   - `FEATURE_ADVANCED_PERMISSIONS` is explicitly set to `false` until migration to v2 role engine is completed.
2. **Custom S3 Endpoint**:
   - Uses EU region S3 bucket (`s3.eu-central-1.amazonaws.com/beta-logistics-storage`).

---

## Template for New Tenant Entry

```markdown
## [Tenant Name] (`[tenant_id]`)

### Environment: [Development | Staging | Production]
- **Tenant ID**: `tnt_...`
- **Slug**: `...`
- **Primary Admin**: `user@example.com`

### Custom Overrides & Notes
1. **[Area 1 - Auth / DB / Storage]**:
   - Notes...
2. **[Area 2 - Feature Flags / Rate Limits]**:
   - Notes...

### Tags
#tenant-specific #tenant-id
```
