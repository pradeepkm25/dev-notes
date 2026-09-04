# Security & Secret Sanitization Policy

This repository is designed for technical knowledge and code snippets. Under no circumstances should sensitive credentials or private corporate data be committed.

---

## 🚫 Prohibited Content (Never Commit!)

Do not commit any of the following to this repository:
- **Access Tokens & Refresh Tokens** (OAuth2 JWTs, bearer tokens).
- **Passwords & Hashes** (database passwords, admin user credentials).
- **Client Secrets & API Keys** (Keycloak client secrets, AWS IAM keys, Sendgrid/Stripe keys).
- **Private Certificates & Keys** (`*.pem`, `*.key`, `*.p12`, `*.crt`).
- **Environment Files** (`.env`, `.env.local`, `.env.production`).
- **Production Endpoints with Sensitive Parameters**.
- **Customer Confidential Information & PII** (real customer names, live user email databases, phone numbers).
- **Internal Proprietary Infrastructure URLs** that must remain confidential.

---

## 🛡️ Required Placeholders

Always use generic, identifiable placeholder strings when documenting curl commands, configs, or queries:

| Sensitive Item | Approved Placeholder |
| :--- | :--- |
| Bearer / Access Token | `YOUR_ACCESS_TOKEN` |
| Refresh Token | `YOUR_REFRESH_TOKEN` |
| Client Secret | `YOUR_CLIENT_SECRET` |
| Password | `YOUR_PASSWORD` |
| Tenant Identifier | `YOUR_TENANT_ID` (e.g., `tnt_abc123`) |
| Tenant Domain / Slug | `acme-corp` or `example-tenant` |
| Host / Base URL | `api.example.com` or `auth.example.com` |
| User Email | `admin@example.com` or `user@example.com` |
| Database Password | `YOUR_DB_PASSWORD` |

---

## 🔍 Pre-Commit Sanitization Check

Before committing, run a quick grep search to confirm no accidental leaks:
```powershell
git diff | Select-String -Pattern "eyJh", "BEGIN PRIVATE", "password=", "client_secret="
```

If a secret is ever accidentally committed:
1. Immediately rotate / revoke the compromised credential in the target system.
2. Purge the secret from Git history using `git-filter-repo` or BFG Repo-Cleaner before pushing.
