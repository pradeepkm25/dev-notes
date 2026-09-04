# Feature Completion Checklist

Practical checklist to run through before closing a development task, opening a pull request, or marking a user story as complete.

---

## 🎯 Knowledge Capture

- [ ] Did I discover something reusable?
- [ ] Can another tenant use this?
- [ ] Did I find a useful API?
- [ ] Did I create a useful SQL query?
- [ ] Did I solve a reusable problem?
- [ ] Did I discover an automation opportunity?
- [ ] Did I update the Knowledge Base?

---

## 🔒 Security & Code Quality

- [ ] Did I sanitize all secrets, passwords, tokens, and private credentials before committing?
- [ ] Are backend authorization guards (`@PreAuthorize`) implemented for all new mutating endpoints?
- [ ] Did I verify multi-tenant isolation (confirming user cannot access other tenant data)?
- [ ] Did I write automated tests covering the happy path, missing parameters, and error responses?

---

## 🏷️ Tags
#checklist #feature-completion #definition-of-done #knowledge-base #quality
