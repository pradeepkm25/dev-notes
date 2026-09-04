# End-to-End API Flows

Sequence flows for complex multi-step interactions between frontend, API gateways, Keycloak, and microservices.

---

## 🔄 Flow 1: User Login & Tenant Context Resolution

```text
[ Browser / Client ]              [ Keycloak ]               [ API Gateway / Backend ]
         │                             │                                 │
         │─── 1. Authenticate ────────>│                                 │
         │    (OAuth2 / OIDC Auth Code)│                                 │
         │                             │                                 │
         │<── 2. Return JWT Token ─────│                                 │
         │    (Contains tenant_id)     │                                 │
         │                                                               │
         │─── 3. Request Profile /api/v1/tenants/current ───────────────>│
         │       (Authorization: Bearer <JWT>)                           │── Validate JWT Signature
         │                                                               │── Extract Tenant Context
         │                                                               │── Attach to ThreadLocal
         │<── 4. Return Tenant Profile & Active Permissions ─────────────│
```

---

## 🔄 Flow 2: Multi-Tenant Batch Ingestion Flow

```text
[ Data Steward ]                     [ API Service ]                    [ DB / Storage ]
       │                                    │                                  │
       │── 1. POST /entities/batch/upload ─>│                                  │
       │      (Multipart CSV payload)       │── Validate CSV Headers & Schema  │
       │                                    │── Check 'data:import:batch'      │
       │                                    │── Stage file to S3 tenant path ─>│
       │<─ 2. Return 202 Accepted (Job ID) ─│                                  │
       │                                    │                                  │
       │                                    │── Async Ingestion Worker ───────>│ Batch Insert Records
       │                                    │                                  │ with tenant_id key
       │── 3. GET /batch/jobs/{jobId} ─────>│                                  │
       │<─ 4. Return Status: COMPLETED ─────│                                  │
```

---

## 🏷️ Tags
#api #flows #architecture #sequence #multi-tenant
