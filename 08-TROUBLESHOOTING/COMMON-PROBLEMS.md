# Common Problems & Quick Solutions

A rapid lookup cheat sheet for day-to-day development gotchas across environments.

---

## ⚡ Quick Problem-Solution Lookup

| Symptom | Probable Cause | Immediate Action |
| :--- | :--- | :--- |
| `CORS error: No 'Access-Control-Allow-Origin'` | Missing frontend origin in Keycloak Web Origins | Add `+` or frontend URL to Client Web Origins in Keycloak |
| `Keycloak: Invalid parameter: redirect_uri` | Exact redirect URL not whitelisted in Client | Add exact URL pattern to **Valid Redirect URIs** |
| `HikariPool connection timeout` | Unclosed `@Transactional` or connection leak | Inspect active queries via `pg_stat_activity` and cancel blockers |
| `403 Forbidden` on newly added API | User role missing newly mapped permission | Update `role_permissions` mapping table in DB / Keycloak scope |
| `Flyway schema version mismatch` | Local checksum mismatch on modified migration | Run `flyway repair` locally or restore original SQL migration file |
| `Docker container cannot connect to Postgres` | Using `localhost` instead of `host.docker.internal` | Change DB host to `host.docker.internal` in container `.env` |

---

## 🏷️ Tags
#troubleshooting #common-problems #cheat-sheet #quick-fixes
