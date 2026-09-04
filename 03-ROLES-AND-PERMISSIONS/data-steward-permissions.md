# Data Steward — CREATE and EDIT_ALL Permissions

**Date:** 2026-09-04  
**Module:** Roles and Permissions  
**Category:** Tenant Configuration / Role Setup  

---

## 🎯 Purpose

Document the required permissions for the **Data Steward** role so that the same configuration can be correctly applied to future tenants.

---

## 🔍 Discovery

While configuring permissions for the Data Steward role, it was identified that providing only one permission is not sufficient for the required functionality.

The Data Steward role requires both:

* `CREATE`
* `EDIT_ALL`

---

## 🛡️ Required Permissions

| Permission | Purpose |
| :--- | :--- |
| `CREATE` | Allows the Data Steward to create new records or entities where applicable. |
| `EDIT_ALL` | Allows the Data Steward to edit all permitted records or entities. |

---

## ⚠️ Important Rule

> When configuring the Data Steward role, ensure that both `CREATE` and `EDIT_ALL` permissions are assigned.

Do not assume that `EDIT_ALL` automatically provides the required create functionality.

---

## 📌 Where This Applies

This should be checked for:

* [x] New tenant setup
* [x] Tenant role configuration
* [x] Existing tenants with incomplete Data Steward permissions
* [x] Future tenant provisioning automation

---

## 🔄 Recommended Flow

```text
Create / Configure Tenant
        ↓
Create or Verify Data Steward Role
        ↓
Assign CREATE Permission
        ↓
Assign EDIT_ALL Permission
        ↓
Assign Role to User
        ↓
Test Create Functionality
        ↓
Test Edit Functionality
        ↓
Verify Access
```

---

## ✅ Validation Checklist

Before considering the Data Steward configuration complete:

* [ ] Data Steward role exists.
* [ ] `CREATE` permission is assigned.
* [ ] `EDIT_ALL` permission is assigned.
* [ ] The permissions are correctly mapped to the role.
* [ ] A test user is assigned the Data Steward role.
* [ ] Create functionality is tested.
* [ ] Edit functionality is tested.

---

## 🧠 Reusable Learning

When configuring roles and permissions, do not assume that one broad permission automatically includes all related operations.

Always verify each required action individually:

* Create
* Read
* Edit
* Delete
* Other module-specific actions

---

## 💡 Future Enhancement Opportunity

Consider automating the default Data Steward permission setup during tenant provisioning.

Possible automation:

```text
New Tenant Created
        ↓
Default Roles Created
        ↓
Data Steward Role Identified
        ↓
Automatically Assign:
  - CREATE
  - EDIT_ALL
        ↓
Validate Role Configuration
```

This can help ensure consistent configuration across future tenants.

---

## 🏷️ Tags

#roles #permissions #data-steward #tenant #tenant-provisioning #create #edit-all #reusable
