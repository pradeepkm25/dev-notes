# Akashic Data Platform (ADP)

**Module:** ADP / Data Platform  
**Repository:** `akashic-backend-nest`  

---

## 🎯 Scope & Responsibilities
- Core backend platform services, background worker orchestration, and business domain endpoints.
- Integration gateway connecting frontend web clients with microservice backends.

---

## ⚡ Key Architectural Patterns
- **Tenant Context Resolution**: Intercepts requests, validates JWT claims (`tenant_id`, `tenant_name`), and populates execution context.
- **Async Job Queue**: Dispatches asynchronous processing jobs (ingestion, export, notifications) via queue workers.

---

## ⚡ Common Operations & Gotchas
- **Thread/Context Leaks**: Always invoke `TenantContext.clear()` in NestJS interceptors / middleware `finally` blocks.
- **Service-to-Service Calls**: Use `akashic-system` client credentials token when invoking downstream internal microservices.

---

## 🏷️ Tags
#adp #data-platform #backend #nestjs #tenant-context #async-workers
