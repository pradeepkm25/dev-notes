# API Testing Checklist

Comprehensive test verification checklist for reviewing REST APIs prior to staging or production deployments.

---

## 🧪 API Verification Gate

### 1. Authentication
- [ ] Valid Bearer token returns expected response (`200 OK` / `201 Created`).
- [ ] Missing token returns `401 Unauthorized`.
- [ ] Malformed or expired token returns `401 Unauthorized`.

### 2. Authorization & RBAC
- [ ] Role possessing required permission executes successfully.
- [ ] Role lacking permission receives `403 Forbidden` with clean JSON error body.
- [ ] Frontend and backend guards are synchronized.

### 3. Success Response & Contract
- [ ] Response payload matches OpenAPI / Swagger schema contract.
- [ ] Pagination parameters (`page`, `size`, `sort`) return correct offsets and totals.
- [ ] Timestamps are formatted in ISO-8601 UTC (`YYYY-MM-DDTHH:mm:ssZ`).

### 4. Invalid & Missing Input Validation
- [ ] Empty request body on POST returns `400 Bad Request`.
- [ ] Missing mandatory fields return descriptive field validation error list.
- [ ] Invalid data types (e.g. string sent for integer) return `400 Bad Request`.

### 5. Edge Cases & Safety
- [ ] Extremely large payload triggers `413 Payload Too Large` or handles streaming cleanly.
- [ ] Special characters and SQL injection attempts are safely parameterized and escaped.
- [ ] Concurrent requests on idempotent operations do not create duplicate records.

### 6. Error Responses
- [ ] Error responses never leak stack traces or internal server IP addresses.
- [ ] Standard error format: `{ errorCode, message, timestamp, details }`.

### 7. Tenant-Specific Behavior & Isolation
- [ ] Tenant A cannot access, query, or mutate Tenant B's data (`403 Forbidden` or `404 Not Found`).
- [ ] Tenant-specific feature flags are respected (disabled features return `403` or disabled state).

---

## 🏷️ Tags
#checklist #api #testing #security #validation #quality
