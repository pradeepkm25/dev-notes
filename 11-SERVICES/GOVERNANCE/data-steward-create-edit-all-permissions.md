# Governance: Data Steward `CREATE` and `EDIT_ALL` Permissions

**Module:** Governance / Roles & Permissions  
**Cross-Reference:** [`03-ROLES-AND-PERMISSIONS/data-steward-permissions.md`](file:///d:/Dhira-Work/dev-notes/03-ROLES-AND-PERMISSIONS/data-steward-permissions.md)  

---

## 🎯 Summary
In the Governance module, Data Stewards curate datasets, metadata schemas, and business glossaries.

When assigning permissions to the Data Steward role in Governance:
* `CREATE` is required to author new catalog entities, datasets, and taxonomies.
* `EDIT_ALL` is required to edit, curate, or enrich existing datasets created across all tenant users.

> [!IMPORTANT]
> Always assign **both** `CREATE` and `EDIT_ALL` permissions to the Data Steward role in Governance. `EDIT_ALL` alone does not grant creation rights.

---

## 🏷️ Tags
#governance #data-steward #permissions #create #edit-all #catalog
