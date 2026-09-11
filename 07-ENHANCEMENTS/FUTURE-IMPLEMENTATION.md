# Future Implementation Tracker

Master tracker of actionable enhancements, automation ideas, and reusable features discovered during active development to be scheduled and implemented later.

---

## 📋 Enhancement Backlog

| ID | Enhancement | Module | Applicable To | Priority | Status | Next Action |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `ENH-001` | Multi-Tenant S3 Pre-Signed Uploads | Storage Service | All Tenants | High | Ready | Implement `/storage/presign` endpoint in file-service |
| `ENH-002` | Keycloak Automated Tenant Client Provisioning | IAM / Provisioning | All Tenants | High | Planned | Write script using Keycloak Admin REST API |
| `ENH-003` | Self-Service Custom Domain SSL Provisioning | Gateway / Routing | Enterprise Tenants | High | Needs Discussion | Evaluate Caddy vs AWS ACM automated DNS challenge |
| `ENH-004` | Audit Log Automated Export to S3 | Security / Audit | All Tenants | Medium | Idea | Design nightly S3 export lambda/cron worker |
| `ENH-005` | HikariCP Connection Pool Warmup | Database Layer | High-Traffic Tenants | Low | Idea | Benchmark pod startup latency improvements |
| `ENH-006` | Keycloak Realm Brute Force Detection | Security / Provisioning | All Tenants | Medium | Planned | Apply `REALM_BRUTE_FORCE_CONFIG` in `PlatformRealmService` |

---

## 🏷️ Status Definitions

- **Idea**: Initial thought captured; requires elaboration.
- **Planned**: Scoped and scheduled for upcoming development cycle.
- **In Progress**: Actively being implemented on a branch.
- **Blocked**: Awaiting dependencies, design decisions, or infrastructure access.
- **Implemented**: Finished, tested, and documented in [`IMPLEMENTED-ENHANCEMENTS.md`](file:///d:/Dhira-Work/dev-notes/07-ENHANCEMENTS/IMPLEMENTED-ENHANCEMENTS.md).
- **Archived**: Deemed unnecessary or superseded.

---

## 🏷️ Tags
#tracker #backlog #enhancements #roadmap
