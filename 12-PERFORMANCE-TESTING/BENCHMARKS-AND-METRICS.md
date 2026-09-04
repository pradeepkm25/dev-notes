# Performance Benchmarks & SLA Thresholds

Standard latency SLAs, throughput targets, and resource utilization benchmarks for multi-tenant backend services.

---

## 📊 Service SLA & Target Thresholds

| Operation Type | Endpoint / Scope | Target Throughput (RPS) | p95 Latency | p99 Latency | Max Error Rate |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Token Exchange** | Keycloak `/token` | 150 req/sec | < 150 ms | < 300 ms | < 0.1% |
| **Tenant Metadata Lookup** | Tenant Admin `/tenants/{id}` | 500 req/sec | < 50 ms | < 100 ms | < 0.05% |
| **Catalog Query (Cached)** | AMD `/entities` | 300 req/sec | < 80 ms | < 150 ms | < 0.1% |
| **Batch CSV Ingest (Async)** | ADP `/entities/batch` | 20 jobs/min | < 2.0 s (202 Accepted) | < 5.0 s | < 0.5% |
| **BI Query Execution** | BI `/analytics/query` | 50 req/sec | < 400 ms | < 1.0 s | < 1.0% |

---

## 🔍 Bottleneck Checklist

- [ ] **DB Connection Pool (HikariCP)**: Max pool size not saturated (`activeConnections < maxPoolSize`).
- [ ] **PostgreSQL Slow Query Log**: No queries exceeding 250ms without index utilization.
- [ ] **Keycloak CPU Utilization**: CPU < 70% during peak login load.
- [ ] **Node.js / NestJS Event Loop Delay**: Event loop delay < 20ms during I/O bursts.

---

## 🏷️ Tags
#benchmarks #sla #metrics #latency #p95 #p99 #performance-testing
