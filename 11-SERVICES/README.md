# 11 - Microservices & Application Modules

Centralized notes, architecture overviews, inter-service communication patterns, and configurations across the platform microservices.

---

## 📚 Service Modules

| Module / Service | Key Responsibility | Primary Tech / Dependencies | Quick Note Link |
| :--- | :--- | :--- | :--- |
| **Governance** | Data governance, policy enforcement, bot tokens, audit | NestJS, PostgreSQL, Keycloak | [`GOVERNANCE.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/GOVERNANCE.md) |
| **BI (Business Intelligence)** | Analytics, reporting dashboards, cube queries | Superset / Metabase, Cube, Keycloak | [`BI.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/BI.md) |
| **ADW (Data Warehouse)** | Warehouse schema, ETL pipelines, analytics queries | PostgreSQL / ClickHouse / BigQuery | [`ADW.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/ADW.md) |
| **ADP (Data Platform)** | Ingestion pipelines, dataset orchestration, workers | NestJS, Kafka/RabbitMQ, S3 | [`ADP.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/ADP.md) |
| **AMD (Master Data)** | Master data catalog, entity schemas, taxonomies | NestJS, TypeORM/Prisma, PostgreSQL | [`AMD.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/AMD.md) |
| **Tenant Admin Service** | Tenant metadata, feature flag controls, tenant user admin | NestJS, Keycloak Admin API, PostgreSQL | [`TENANT-ADMIN.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/TENANT-ADMIN.md) |
| **Tenant Provisioning** | Automated realm/client creation, schema init, seed data | NestJS, Keycloak Admin Client, PostgreSQL | [`TENANT-PROVISIONING.md`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/TENANT-PROVISIONING.md) |
