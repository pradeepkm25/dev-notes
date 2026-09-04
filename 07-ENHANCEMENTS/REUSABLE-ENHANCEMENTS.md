# Reusable Enhancements

A structured repository of architectural patterns, reusable modular components, and enhancements discovered during software development.

---

## ENH-001 — Dynamic Multi-Tenant S3 Pre-Signed Upload Generator

**Date:** 2026-09-04  
**Module:** Storage & File Service  
**Discovered During:** Large batch CSV ingestion implementation  

### Problem / Opportunity
Direct file streaming through backend microservices causes high CPU spikes, memory saturation, and occasional gateway timeouts (504) during multi-megabyte uploads.

### Proposed Enhancement
Implement a dedicated pre-signed URL generation endpoint that generates a short-lived AWS S3 upload URL. The URL strictly enforces tenant isolation prefixing: `tenants/{tenant_id}/uploads/{uuid}/{filename}`.

### Why It Is Reusable
Any tenant or service module handling file uploads (documents, catalog feeds, avatar images) can reuse this exact flow to offload bandwidth to cloud storage without compromising security.

### Applicable To
- [x] Tenant Ingestion Module
- [x] Document Management Service
- [x] All future tenants

### Implementation Notes
1. Create `/api/v1/tenants/{tenant_id}/storage/presign` endpoint.
2. Sign with S3 PutObject permission, 15-minute expiry, and content-length boundary.
3. Client uploads directly to S3 and triggers callback with S3 object key.

### Related APIs / Database / Configuration
- **APIs**: `POST /api/v1/tenants/{id}/storage/presign`
- **Configuration**: `AWS_S3_BUCKET_NAME`, `AWS_REGION`

### Priority
High

### Status
Ready

### Tags
#enhancement #reusable #s3 #storage #performance

---

## ENH-002 — Automated Keycloak Tenant Client & Mapper Provisioning

**Date:** 2026-09-04  
**Module:** Tenant Provisioning / IAM  
**Discovered During:** Manual tenant onboarding  

### Problem / Opportunity
Creating Keycloak clients, setting protocol mappers, and assigning client roles manually via the admin UI takes ~20 minutes per tenant and is prone to missed mappers.

### Proposed Enhancement
Build a Java/Node provisioning script that leverages the Keycloak Admin REST API to instantiate client configurations from a JSON template in one call.

### Why It Is Reusable
Runs on every new tenant provisioning trigger and ensures 100% configuration consistency.

### Applicable To
- [x] Automated tenant provisioning worker
- [x] CI/CD tenant staging setup

### Implementation Notes
Integrate into the tenant onboarding pipeline post database migration.

### Related APIs / Database / Configuration
- **APIs**: Keycloak Admin API `/admin/realms/{realm}/clients`
- **Database**: `app_master.tenants`

### Priority
High

### Status
Planned

### Tags
#enhancement #keycloak #automation #tenant-provisioning
