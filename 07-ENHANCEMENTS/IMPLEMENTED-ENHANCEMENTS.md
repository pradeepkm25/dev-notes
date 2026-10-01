# Implemented Enhancements

Catalog of successfully completed reusable enhancements, architecture patterns, and retrospective learnings.

---

## ENH-000 — Universal Tenant Context Security Filter

### Problem
Tenant validation and extraction was previously duplicated inside each individual controller method, resulting in boilerplate and occasional authorization bypass risks.

### Solution Implemented
Created a centralized `TenantContextFilter` servlet filter that intercepts all incoming requests, extracts `tenant_id` from the verified JWT claim, and stores it in a `ThreadLocal` context wrapper accessible throughout the request lifecycle.

### Where It Was Implemented
- Backend Core Security Module (`common-security-lib`)
- Applied across all REST microservices

### Reusable Learning
Always provide a clean `try-finally` block that invokes `TenantContextHolder.clear()` in the filter to prevent context bleed across pooled web server threads.

### Date Implemented
2026-08-20

---

## ENH-006 — Automated Keycloak Realm Brute Force Detection

### Problem
Newly provisioned tenant realms were vulnerable to automated dictionary and credential-stuffing attacks if brute force defenses were left unconfigured.

### Solution Implemented
Integrated `REALM_BRUTE_FORCE_CONFIG` into `PlatformRealmService` in `akashic-tenant-provisioning-api` to automatically enforce temporary lockout protection (10 max failures, 15 min max wait, 12 hr failure reset) during tenant realm creation and configuration.

### Where It Was Implemented
- `akashic-tenant-provisioning-api` (`PlatformRealmService.createRealm`)
- Applied across all new and existing tenant realms

### Reusable Learning
Use `permanentLockout: false` to lockout users temporarily (exponential backoff) rather than permanently, preventing unnecessary operational support tickets for manual account unlocking.

### Date Implemented
2026-10-01

---

## 🏷️ Tags
#implemented #enhancements #keycloak #security #brute-force #tenant-provisioning
