# Performance Testing Guide & Load Test Scenarios

Practical load testing methodology, tool configurations (k6), and multi-tenant performance test workflows.

---

## 🎯 Test Scenarios

### 1. Concurrent Tenant Login & Token Acquisition
- **Goal**: Measure Keycloak token endpoint throughput under load.
- **Scenario**: 100 concurrent Virtual Users (VUs) requesting tokens via Password / Client Credentials grant.

### 2. Multi-Tenant Batch Ingestion
- **Goal**: Measure ADP / AMD service CPU & DB connection pool limits during bulk CSV ingestion.
- **Scenario**: 20 simultaneous tenants uploading 5,000 entities each.

### 3. Read-Heavy Catalog & Dashboard Queries
- **Goal**: Verify p95 latency under 200ms with cached vs uncached requests.

---

## ⚡ Quick k6 Script Template (`load-test.js`)

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '30s', target: 20 },  // Ramp-up
    { duration: '1m', target: 50 },   // Steady load
    { duration: '20s', target: 0 },   // Ramp-down
  ],
  thresholds: {
    http_req_duration: ['p(95)<200'], // 95% requests must finish under 200ms
    http_req_failed: ['rate<0.01'],   // <1% errors
  },
};

export default function () {
  const params = {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json',
    },
  };

  const res = http.get('https://api.example.com/api/v1/tenants/YOUR_TENANT_ID/entities', params);
  check(res, {
    'status is 200': (r) => r.status === 200,
  });

  sleep(1);
}
```

---

## 🏷️ Tags
#performance-testing #load-testing #k6 #benchmarks #concurrency #latency
