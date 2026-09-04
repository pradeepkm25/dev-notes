# Automation Opportunities

Catalog of manual, repetitive development steps, deployment tasks, or tenant onboarding activities identified for automated scripting.

---

## AUTO-001 — One-Click Tenant Seed & Keycloak Client Creation

### Manual Process
Currently, provisioning a tenant requires running SQL insert statements manually, navigating the Keycloak Admin UI to create a client, creating protocol mappers for `tenant_id`, and generating initial user invite links.

### Proposed Automation
Create a CLI tool or backend admin endpoint `POST /api/v1/system/tenants/bootstrap` that orchestrates database seeding, schema creation, Keycloak client provisioning, and email dispatch in a single atomic transaction.

### Benefit
Reduces tenant onboarding time from 30 minutes to under 15 seconds; eliminates human configuration mistakes.

### Applicable Scope
Platform Engineering, Support Ops, and automated integration test pipelines.

### Estimated Complexity
Medium

### Status
Planned

---

## AUTO-002 — Automated Periodic S3 Inactive Tenant Archiving

### Manual Process
Storage quotas for suspended or trial-ended tenants are currently reviewed and cleaned manually on AWS S3 console.

### Proposed Automation
Deploy an AWS S3 Lifecycle rule combined with an EventBridge scheduled task that automatically tags and moves `/tenants/{suspended_tenant_id}/` storage objects to S3 Glacier Deep Archive after 90 days.

### Benefit
Reduces cloud storage costs automatically without risking premature data deletion.

### Applicable Scope
All tenants across multi-tenant production buckets.

### Estimated Complexity
Low

### Status
Idea

---

## 🏷️ Tags
#automation #efficiency #tenant-provisioning #devops #scripting
