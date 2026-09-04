# DBeaver Timezone Configuration Issue

**Date:** 2026-09-04  
**Module:** Database / DBeaver  
**Category:** Troubleshooting  

---

## 🛑 Problem

The displayed date or time in DBeaver is incorrect due to an incorrect client interface timezone configuration.

### ISSUE-001 (Tooling) — Incorrect Timezone in DBeaver

**Problem:** Date/time values display with an incorrect timezone offset when querying timestamps.

**Quick Fix:**
```text
Window → Preferences → User Interface → Timezone → Asia/Kolkata
```

---

## 🔧 Solution

Configure the timezone in DBeaver using the following navigation path:

```text
Window
  → Preferences
    → User Interface
      → Timezone
        → Asia/Kolkata
```

Select:
```text
Asia/Kolkata
```

---

## ✅ Validation

After changing the timezone:

* [ ] Apply or save the changes.
* [ ] Restart DBeaver if required.
* [ ] Reconnect to the database.
* [ ] Verify that timestamps are displayed correctly.

---

## ⚠️ Important Note

If timestamps still appear incorrect after changing the DBeaver interface timezone, also check:

* **Database Server Timezone**: `SHOW timezone;` or server-level setting.
* **Database Session Timezone**: `SET TIME ZONE 'Asia/Kolkata';`.
* **Application Timezone Configuration**: JVM `-Duser.timezone=UTC` or application properties.
* **Timestamp Column Type**: Difference between `TIMESTAMP` (without timezone) vs `TIMESTAMPTZ` (with timezone).

> [!NOTE]
> The DBeaver timezone setting affects how date and time values are rendered in the client UI, while the database engine itself maintains separate server and session timezone settings.

---

## ⚡ Quick Navigation Summary

```text
Window → Preferences → User Interface → Timezone → Asia/Kolkata
```

---

## 🏷️ Tags

#dbeaver #database #timezone #troubleshooting #asia-kolkata #sql
