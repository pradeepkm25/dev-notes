# Lessons Learned & Engineering Takeaways

Reflections, architectural takeaways, and engineering insights gathered from real-world software delivery.

---

# LESSON-001 — Enforce Tenant Isolation Centrally in Gateway Security Filters

## Situation
During an early multi-tenant sprint, tenant extraction and verification logic was implemented ad-hoc inside individual microservice controllers.

## What Happened
A newly added service endpoint omitted the check confirming that the path parameter `/tenants/{id}` matched the JWT token's `tenant_id` claim. This briefly exposed an authorization bypass risk during security review.

## What I Learned
Tenant context extraction, signature validation, and cross-validation between URLs and JWT claims must happen centrally at the API Gateway / Security Filter level before requests reach controllers.

## What I Will Do Differently
- Always inject tenant context into a global `TenantContextHolder` via standard middleware.
- Ban direct reading of path-based tenant IDs inside business logic services.

## Where This Applies
All REST microservices, GraphQL resolvers, and async event consumers.

## Tags
#lessons-learned #architecture #security #multi-tenant

---

# LESSON-002 — PostgreSQL DDL Migrations on High-Volume Multi-Tenant Tables

## Situation
Attempted to run a Flyway migration that added a `NOT NULL` column with a default value to a multi-million-row PostgreSQL table.

## What Happened
The migration took an exclusive table lock for over 15 minutes, blocking all tenant queries and triggering cascading connection pool timeouts.

## What I Learned
Running unvalidated `ALTER TABLE ... ADD COLUMN ... NOT NULL DEFAULT` locks large tables in PostgreSQL.

## What I Will Do Differently
1. Add the column as `NULL` without default.
2. Backfill existing records in asynchronous batches of 1,000 rows.
3. Add the `NOT NULL` constraint using `CHECK ... NOT VALID`, followed by `VALIDATE CONSTRAINT` to prevent table locking.

## Where This Applies
All production relational database schema migrations.

## Tags
#lessons-learned #database #postgresql #migrations #performance
