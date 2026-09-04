# 10 - Archive

Repository of deprecated, legacy, or superseded documentation, architecture decisions, and historical notes.

---

## 🎯 Purpose

- **Preserve Context**: We do not permanently delete technical history. When a flow, API version, or tenant setup pattern becomes obsolete, move the markdown file here.
- **Clean Active Navigation**: Keeps the main numbered folders (`02-TENANTS` through `09-CHECKLISTS`) focused strictly on active, modern production systems.

---

## 🗄️ How to Archive a Note

1. Move the markdown file into this directory:
   `git mv 02-TENANTS/old-flow.md 10-ARCHIVE/2026-old-flow.md`
2. Add a deprecation banner at the top of the file:
   ```markdown
   > [!WARNING]
   > **DEPRECATED (YYYY-MM-DD)**: This document is preserved for historical reference only. 
   > Superseded by [NEW-DOC](../02-TENANTS/NEW-FLOW.md).
   ```
3. Update `INDEX.md` and `CHANGELOG.md` to reflect the move.
