# Table Relationships & Schema Architecture

Overview of core database entities, foreign key references, and multi-tenant partitioning conventions.

---

## 🗺️ Entity Relationship Diagram

```mermaid
erDiagram
    TENANTS ||--o{ TENANT_SETTINGS : has
    TENANTS ||--o{ USERS : owns
    TENANTS ||--o{ ENTITIES : contains
    USERS ||--o{ USER_ROLES : assigned
    ROLES ||--o{ USER_ROLES : defines
    ROLES ||--o{ ROLE_PERMISSIONS : maps
    PERMISSIONS ||--o{ ROLE_PERMISSIONS : granted
    ENTITIES ||--o{ ENTITY_ATTRIBUTES : has
    ENTITIES ||--o{ AUDIT_LOGS : generates

    TENANTS {
        string tenant_id PK
        string slug
        string name
        string status
        timestamp created_at
    }

    USERS {
        string id PK
        string tenant_id FK
        string email
        string status
        timestamp created_at
    }

    ENTITIES {
        string id PK
        string tenant_id FK
        string name
        string category
        string status
        string created_by FK
        timestamp created_at
        timestamp updated_at
    }
```

---

## 🔒 Multi-Tenant Data Isolation Principles

1. **Discriminator Column**: Every domain table MUST contain a non-nullable `tenant_id VARCHAR(64)` column.
2. **Compound Indexes**: Composite primary keys or indexes should lead with `tenant_id` for efficient indexing (e.g., `INDEX idx_entities_tenant_category (tenant_id, category)`).
3. **Foreign Key Restraints**: Cross-tenant relationships are strictly forbidden. Foreign keys must refer to parent tables within the same tenant scope.

---

## 🏷️ Tags
#database #schema #er-diagram #foreign-keys #multi-tenant
