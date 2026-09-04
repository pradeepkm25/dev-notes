# Akashic Master Data (AMD)

**Module:** AMD / Master Data  
**Repository:** `akashic-amd-api`  

---

## 🎯 Scope & Responsibilities
- Master data catalog, taxonomy definitions, entity attribute mappings, and shared reference data.
- Catalog publishing, data curation, and Data Steward operations.

---

## 🛡️ Role & Permission Dependencies
- **Data Steward Operations**: Data Stewards require both `data:create` and `data:edit:all` to curate shared catalog entities (see [`data-steward-permissions.md`](file:///d:/Dhira-Work/dev-notes/03-ROLES-AND-PERMISSIONS/data-steward-permissions.md)).
- **Batch Upload**: Endpoints on AMD require streaming CSV parsers and pre-signed S3 upload URLs to handle high entity counts.

---

## ⚡ Common Operations & Gotchas
- **Attribute Caching**: Cache common reference taxonomies per tenant in Redis; invalidate cache on master record updates.
- **Foreign Key Restraints**: Ensure category/entity deletions follow soft-delete flows in [`DELETE-FLOWS.md`](file:///d:/Dhira-Work/dev-notes/05-DATABASE/DELETE-FLOWS.md).

---

## 🏷️ Tags
#amd #master-data #catalog #data-steward #taxonomies
