# 11 - Microservices & Application Modules

Dedicated modular directories for platform microservices, technical domain workflows, service-specific permission patterns, and configuration guides.

---

## 📁 Service Directories

| Service Folder | Key Responsibility | Primary Tech / Dependencies | Link |
| :--- | :--- | :--- | :--- |
| **`GOVERNANCE/`** | Data governance, policy enforcement, Data Steward `CREATE`/`EDIT_ALL` permissions, bot tokens | NestJS, PostgreSQL, Keycloak | [`GOVERNANCE/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/GOVERNANCE/) |
| **`BI/`** | Embedded analytics, reporting dashboards, cube query isolation | Superset / Metabase, Keycloak | [`BI/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/BI/) |
| **`ADW/`** | Data Warehouse schemas, analytical ETL, multi-tenant partitioning | PostgreSQL / ClickHouse | [`ADW/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/ADW/) |
| **`ADP/`** | Data Platform backend core, ingestion workers, tenant context filters | NestJS, Redis/Queues, S3 | [`ADP/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/ADP/) |
| **`AMD/`** | Master data catalog, entity taxonomies, shared reference curation | NestJS, PostgreSQL | [`AMD/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/AMD/) |
| **`TENANT-ADMIN/`** | Tenant settings, dynamic feature flags, tenant user management | NestJS, Keycloak Admin API | [`TENANT-ADMIN/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/TENANT-ADMIN/) |
| **`TENANT-PROVISIONING/`** | Realm bootstrapping, service client creation, protocol mappers | NestJS, Keycloak Admin Client | [`TENANT-PROVISIONING/`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/TENANT-PROVISIONING/) |
