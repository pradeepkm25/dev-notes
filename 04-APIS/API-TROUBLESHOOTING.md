# API Troubleshooting Guide

Quick diagnostic workflows for common HTTP error codes and API integration failures.

---

## 🛑 Common HTTP Status Codes & Diagnostics

### 1. `401 Unauthorized`
- **Likely Cause**: Missing `Authorization: Bearer <token>` header, expired JWT token, or token signed with an untrusted public key (realm mismatch).
- **Diagnostic Step**: Decode JWT token at jwt.io or using `jwt-cli` to inspect the `exp` timestamp and issuer (`iss`).
- **Fix**: Re-authenticate via Keycloak token endpoint to acquire a fresh access token.

### 2. `403 Forbidden`
- **Likely Cause**: Token is valid, but user/client lacks the required permission authority (`hasAuthority('...')`) or tenant ID mismatch.
- **Diagnostic Step**: Inspect JWT `realm_access.roles` and `resource_access.<client>.roles` or custom `permissions` claims.
- **Fix**: Grant the missing permission to the user's role in the database/Keycloak.

### 3. `404 Not Found` on Tenant-Scoped Endpoint
- **Likely Cause**: The resource exists in the database, but belongs to another tenant (`tenant_id != current_tenant_id`). The query filter correctly returns 0 rows to preserve tenant isolation.
- **Diagnostic Step**: Check the database row's `tenant_id` column vs the `tenant_id` in the caller's JWT token.

### 4. `429 Too Many Requests`
- **Likely Cause**: Tenant has exceeded rate limiting thresholds (e.g., batch ingestion limit of 10 requests/min).
- **Diagnostic Step**: Inspect `X-RateLimit-Remaining` and `Retry-After` HTTP response headers.
- **Fix**: Implement exponential backoff in client scripts or request rate limit adjustment in `tenant_settings`.

### 5. `504 Gateway Timeout`
- **Likely Cause**: Long-running synchronous operation (such as CSV export or mass DB update) exceeded the ingress timeout (typically 60s).
- **Fix**: Convert the endpoint into an asynchronous job that immediately returns `202 Accepted` with a Job ID.

---

## 🏷️ Tags
#api #troubleshooting #http-status #debugging #rest #rate-limit
