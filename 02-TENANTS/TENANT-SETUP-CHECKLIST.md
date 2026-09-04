# Tenant Setup Checklist

Step-by-step procedure to provision and verify a new tenant across databases, Keycloak, configuration tables, and default roles.

---

## 📋 Pre-Requisites

- [ ] Target environment selected (`DEV`, `STAGING`, `PROD`).
- [ ] Tenant identifier/slug decided (lowercase, alphanumeric + hyphen, e.g., `acme-corp`).
- [ ] Primary Tenant Admin email address obtained.
- [ ] Database credentials with DDL permissions (if schema-per-tenant isolation is used).
- [ ] Keycloak Master / Admin API token or console access.

---

## ⚙️ Step-by-Step Provisioning

### 1. Database & Schema Provisioning
- [ ] **Create Tenant Record**:
  ```sql
  INSERT INTO app_master.tenants (tenant_id, slug, name, status, created_at)
  VALUES ('tnt_abc123', 'acme-corp', 'Acme Corporation', 'ACTIVE', NOW());
  ```
- [ ] **Run Migration / Schema Init**:
  - If schema-per-tenant: Run Liquibase/Flyway targeting schema `tenant_acme_corp`.
  - If shared-schema: Verify `tenant_id` indexes exist on all tenant-isolated tables.
- [ ] **Seed Default Seed Data**:
  - Insert default categories, status codes, and baseline system lookups.

### 2. Keycloak & Identity Setup
- [ ] **Create / Verify Realm or Client**:
  - Client ID: `acme-corp-app` (or shared multi-tenant client with tenant mapper).
  - Valid Redirect URIs: `https://acme-corp.example.com/*`
  - Web Origins: `https://acme-corp.example.com`
- [ ] **Configure Token Mappers**:
  - Ensure `tenant_id` (`tnt_abc123`) and `tenant_slug` (`acme-corp`) are included in JWT token claims.
- [ ] **Provision Initial Tenant Admin User**:
  - Username / Email: `admin@acme-corp.com`
  - Assign Client / Realm Roles: `TENANT_ADMIN`.
  - Send email verification / password reset link.

### 3. Application Configuration & Feature Flags
- [ ] **Insert Tenant Settings**:
  - Review keys in [`TENANT-CONFIGURATION.md`](file:///d:/Dhira-Work/dev-notes/02-TENANTS/TENANT-CONFIGURATION.md).
  - Configure storage bucket path: `/tenants/tnt_abc123/`.
  - Enable required feature modules (e.g., `ENABLE_ADVANCED_ANALYTICS=true`).
- [ ] **Configure Tenant Notification / SMTP Settings** (if custom SMTP is required).

### 4. Verification & Smoke Testing
- [ ] **Login Smoke Test**: Log in via UI as `admin@acme-corp.com`.
- [ ] **Token Inspection**: Decode JWT at jwt.io (or local tool) to verify `tenant_id` and role claims.
- [ ] **Data Isolation Test**: Confirm user cannot query or view records belonging to other tenants.
- [ ] **File Upload Test**: Upload test document and verify destination in cloud storage.
- [ ] **Secondary User Creation**: Create a regular `MEMBER` user via the Tenant Admin console and verify permissions.

---

## 🏷️ Tags
#tenant #provisioning #checklist #keycloak #database
