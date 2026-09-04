# Personal Development Knowledge Base

A simple, fast, and structured GitHub knowledge base to capture, organize, search, and reuse technical knowledge discovered during software development.

---

## 🎯 Purpose

During day-to-day software development, valuable insights often get lost:
* Reusable feature enhancements and architectural blueprints.
* Tenant-specific setup steps and configuration requirements.
* Role and permission requirements across frontend and backend.
* Useful APIs, curl commands, and end-to-end technical flows.
* SQL queries, database relationships, and safe deletion cascades.
* Keycloak configurations, token mappings, and authentication workflows.
* Errors, root causes, and proven solutions.
* Future automation opportunities and lessons learned.

This repository serves as a permanent, searchable memory bank so that you never have to solve the same problem twice.

---

## ⚡ Main Rule

> **Add something to the knowledge base whenever you discover information that may be useful again.**
> 
> *Speed matters*: A quick note in the Inbox takes less than 2 minutes. Capture immediately, organize later.

---

## 🚀 Quick Start Workflow

```text
1. Found something useful during development?
2. Add a quick note to 00-INBOX/INBOX.md (under 2 minutes).
3. Continue development without losing context.
4. Review the note later (e.g., during weekly triage).
5. Move it to the appropriate category folder.
6. Commit and push changes to GitHub.
```

---

## 🧭 Decision Guide

```text
Did I discover something useful?
        │
        ▼
Will I need this again?
        │
   ┌────┴────┐
   │         │
  YES       MAYBE
   │         │
   ▼         ▼
INBOX     INBOX
   │
   ▼
What type is it?
   │
   ├── API → 04-APIS
   ├── SQL → 05-DATABASE
   ├── Permission → 03-ROLES-AND-PERMISSIONS
   ├── Tenant → 02-TENANTS
   ├── Keycloak → 06-KEYCLOAK
   ├── Service (Governance/BI/ADW/ADP/AMD/Tenant Admin/Provisioning) → 11-SERVICES
   ├── Performance / Load Testing → 12-PERFORMANCE-TESTING
   ├── Enhancement → 07-ENHANCEMENTS
   ├── Error/Solution → 08-TROUBLESHOOTING
   └── Important Rule → 01-IMPORTANT-REMINDERS
```

---

## 📁 Repository Navigation

| Folder | Purpose |
| :--- | :--- |
| [`00-INBOX`](file:///d:/Dhira-Work/dev-notes/00-INBOX/) | Rapid, friction-free dump for quick discoveries (<2 min) |
| [`01-IMPORTANT-REMINDERS`](file:///d:/Dhira-Work/dev-notes/01-IMPORTANT-REMINDERS/) | High-value rules and must-not-forget principles |
| [`02-TENANTS`](file:///d:/Dhira-Work/dev-notes/02-TENANTS/) | Tenant onboarding checklists, configs, flows & overrides |
| [`03-ROLES-AND-PERMISSIONS`](file:///d:/Dhira-Work/dev-notes/03-ROLES-AND-PERMISSIONS/) | Role matrix, permissions index, guards & verification |
| [`04-APIS`](file:///d:/Dhira-Work/dev-notes/04-APIS/) | REST reference, curl snippets, sequence flows & API triage |
| [`05-DATABASE`](file:///d:/Dhira-Work/dev-notes/05-DATABASE/) | Useful queries, ER diagrams, safe deletion & DB gotchas |
| [`06-KEYCLOAK`](file:///d:/Dhira-Work/dev-notes/06-KEYCLOAK/) | Auth flows, user management, role mapping & common issues |
| [`07-ENHANCEMENTS`](file:///d:/Dhira-Work/dev-notes/07-ENHANCEMENTS/) | Reusable patterns, backlog tracker, automation ideas |
| [`08-TROUBLESHOOTING`](file:///d:/Dhira-Work/dev-notes/08-TROUBLESHOOTING/) | Error-solution logs, lessons learned & common problems |
| [`09-CHECKLISTS`](file:///d:/Dhira-Work/dev-notes/09-CHECKLISTS/) | Tenant rollout, feature completion, API & DB safety checklists |
| [`10-ARCHIVE`](file:///d:/Dhira-Work/dev-notes/10-ARCHIVE/) | Obsolete, legacy, or superseded documentation |
| [`11-SERVICES`](file:///d:/Dhira-Work/dev-notes/11-SERVICES/) | Governance, BI, ADW, ADP, AMD, Tenant-Admin & Tenant-Provisioning |
| [`12-PERFORMANCE-TESTING`](file:///d:/Dhira-Work/dev-notes/12-PERFORMANCE-TESTING/) | Load testing scenarios, k6 scripts, SLA targets & benchmarks |
| [`templates`](file:///d:/Dhira-Work/dev-notes/templates/) | Ready-to-copy markdown templates for all doc types |

---

## 🔍 Central Index & Guidelines

- **Central Search Index**: [`INDEX.md`](file:///d:/Dhira-Work/dev-notes/INDEX.md)
- **Security & Secret Sanitization**: [`SECURITY.md`](file:///d:/Dhira-Work/dev-notes/SECURITY.md)
- **Contribution & Note Hygiene**: [`CONTRIBUTING.md`](file:///d:/Dhira-Work/dev-notes/CONTRIBUTING.md)
- **Version Changelog**: [`CHANGELOG.md`](file:///d:/Dhira-Work/dev-notes/CHANGELOG.md)
