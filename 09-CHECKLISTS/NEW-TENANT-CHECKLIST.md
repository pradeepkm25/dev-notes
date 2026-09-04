# New Tenant Rollout & Verification Checklist

Complete quality and onboarding checklist before handing over a newly provisioned tenant environment.

---

## 📋 Pre-Launch Tenant Verification

- [ ] **Tenant Configuration Verified**: Confirm master records in `tenants` and all feature flags/quotas in `tenant_settings`.
- [ ] **Required Roles Created**: Verify standard roles (`TENANT_ADMIN`, `DATA_STEWARD`, `MEMBER`, `VIEWER`) are initialized.
- [ ] **Required Permissions Assigned**: Confirm all permission mappings for default roles are populated.
- [ ] **Keycloak Configuration Checked**: Verify realm/client creation, redirect URIs, web origins, and `tenant_id` protocol mappers.
- [ ] **Required Users Tested**: Provision a test user for both `TENANT_ADMIN` and `MEMBER`; confirm login and email flows work.
- [ ] **APIs Verified**: Run smoke tests against core tenant endpoints (CRUD entities, query catalog).
- [ ] **Important Tenant-Specific Configuration Documented**: Record any bespoke domain, SSO provider, or rate limit overrides in [`02-TENANTS/TENANT-SPECIFIC-NOTES.md`](file:///d:/Dhira-Work/dev-notes/02-TENANTS/TENANT-SPECIFIC-NOTES.md).

---

## 🏷️ Tags
#checklist #tenant #rollout #verification #production-readiness
