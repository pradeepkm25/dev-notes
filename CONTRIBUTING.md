# Contribution & Maintenance Guidelines

Guidelines for keeping this personal knowledge base clean, maintainable, searchable, and secure.

---

## 🧭 Core Principles

1. **Capture First, Organize Later**:
   - When in the middle of coding, never disrupt your flow. Add a raw entry to `00-INBOX/INBOX.md` in under 2 minutes.
2. **Concise and Practical**:
   - Avoid long theoretical essays. Focus on runnable code snippets, copy-pasteable curl commands, clear SQL queries, and exact root causes.
3. **Use Standard Templates**:
   - Always draft permanent notes using the relevant file in `templates/`.
4. **Sanitize Everything**:
   - Zero tolerance for leaked credentials, private tokens, passwords, customer emails, or internal proprietary secrets.
5. **Tag Consistently**:
   - Add lowercase hashtags at the bottom of entries (e.g. `#tenant #permissions #sql #keycloak #bug`).
6. **Date Important Discoveries**:
   - Use ISO date format (`YYYY-MM-DD`) to understand when configurations or behaviors were validated.
7. **Prefer Reusable Knowledge**:
   - Transform tenant-specific or ticket-specific fixes into generic architectural patterns.
8. **Archive Instead of Deleting**:
   - Move deprecated or superseded documentation into `10-ARCHIVE/` to preserve historical problem-solving context.

---

## 🏷️ Tagging Standards

Use standardized lowercase tags:
- `#tenant` / `#tenant-provisioning` / `#tenant-admin`
- `#permissions` / `#roles`
- `#api` / `#curl`
- `#database` / `#sql` / `#postgresql`
- `#keycloak` / `#auth` / `#jwt`
- `#governance` / `#bi` / `#adw` / `#adp` / `#amd`
- `#performance-testing` / `#load-testing` / `#k6` / `#benchmarks`
- `#enhancement` / `#reusable`
- `#automation`
- `#troubleshooting` / `#bug` / `#solution`
- `#security`
- `#important`

---

## 📁 File Naming Conventions

- **Main Reference Files & Indexes**: `UPPERCASE-WITH-HYPHENS.md` (e.g., `API-REFERENCE.md`, `TENANT-CONFIGURATION.md`)
- **Individual In-Depth Topics & Sub-Articles**: `lowercase-with-hyphens.md` (e.g., `create-user-flow.md`, `data-steward-permissions.md`)

---

## 🔄 Note Lifecycle

```text
NEW DISCOVERY
      ↓
INBOX (00-INBOX/INBOX.md)
      ↓
REVIEWED & TRIAGED
      ↓
CATEGORIZED (02-TENANTS, 03-PERMISSIONS, 04-APIS, ...)
      ↓
REUSABLE KNOWLEDGE
      ↓
USED IN FUTURE WORK
      ↓
UPDATED WITH NEW FINDINGS
      ↓ (if obsolete)
ARCHIVED (10-ARCHIVE/)
```

---

## ⏱️ Weekly Review Routine (10–15 Minutes)

Once per week:
1. **Inbox Zero**: Review raw entries in `00-INBOX/INBOX.md` and move them to their category.
2. **Update Trackers**: Update statuses in `07-ENHANCEMENTS/FUTURE-IMPLEMENTATION.md`.
3. **Promote Implemented Items**: Move completed ideas to `07-ENHANCEMENTS/IMPLEMENTED-ENHANCEMENTS.md`.
4. **Check Reminders**: Add any high-impact rules to `01-IMPORTANT-REMINDERS/IMPORTANT-REMINDERS.md`.
5. **Archive**: Move outdated notes to `10-ARCHIVE/`.
6. **Git Commit & Push**: Sync changes with GitHub.

---

## 🐙 Knowledge Base vs. GitHub Issues Workflow

| When to use Knowledge Base Notes | When to use GitHub Issues |
| :--- | :--- |
| Documenting a discovery, solution, or lesson | Implementing a code enhancement or fixing a bug |
| Writing a reusable technical flow or curl command | Assigning priority, milestone, or task owner |
| Documenting tenant configuration parameters | Tracking sprint progress and PR reviews |

### Workflow Diagram

```text
Discovery during development
   ↓
Add to Knowledge Base Note (or INBOX)
   ↓
Does it require active implementation?
   │
   ├── No  → Keep as reference documentation
   │
   └── Yes → Create GitHub Issue (e.g., #12)
                 ↓
             Link Issue in Knowledge Base (`GitHub Issue: #12`)
                 ↓
             Implement code in project repository
                 ↓
             Update status in FUTURE-IMPLEMENTATION.md
                 ↓
             Document finished pattern in IMPLEMENTED-ENHANCEMENTS.md
```
