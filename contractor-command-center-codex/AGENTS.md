# Codex instructions — Contractor Command Center

Read `docs/PRODUCT_REQUIREMENTS.md`, `docs/DATA_MODEL.md`, `docs/SECURITY_AND_DEPLOYMENT.md`, and `docs/ACCEPTANCE_TESTS.md` before modifying code. Treat these documents as product requirements; record unresolved design decisions in `docs/DECISIONS.md` and do not silently assume security properties.

## Engineering directives

- Runtime: **Google Apps Script** (server), **HTML Service** and vanilla HTML/CSS/JS (browser), **Google Sheets** as initial transactional store, **Google Drive** for file attachments. Source under `src/`, versioned with Git and deployed with `clasp`. Do not build a separate Node backend.
- UI language Indonesian; currency IDR; timezone default Asia/Jakarta; date formats unambiguous. Mobile-first and accessible. No fake dashboards populated with hardcoded customer metrics.
- Private copy per buying company; no shared multi-tenant customer database. Avoid public/anonymous deployments with unauthenticated privileged endpoints.
- Identity: document and test the authentication approach for personal Gmail accounts and Workspace domains. `Session.getActiveUser().getEmail()` can be blank; do not assume it always identifies a user. Block privileged flows securely if identity cannot be validated. Never trust browser-provided `role`, user email, project ID, or permission lists for authorization.
- Authorize **every** server mutation and data read; perform company/project scope checks. Define user roles: OWNER, ADMIN, PM, SITE, FINANCE. Default-deny permissions. Restrict management/admin actions to appropriate roles.
- Data rules: `approved != ordered != received != paid`; avoid counting commitment as actual disbursement. Cash receipt != invoiced receivable. Monetary quantities are stored as integer rupiah where feasible. Budget actuals should be reconciled with payment or expense ledger records, not merely request status.
- Use UUIDs for IDs and UTC ISO timestamps internally; write human-readable local dates in the UI. Avoid append duplicate on retry (idempotency keys for critical writes). Use `LockService` for critical Sheet writes and sequencing, and maintain immutable event/audit history for approvals and financial changes.
- Validate all server payloads (type, size, referential integrity, state transitions). Treat all spreadsheet strings and uploaded files as untrusted. Protect from XSS and spreadsheet formula injection. Enforce upload MIME/size allowlist and explicit Drive folder ownership.
- Keep the Apps Script layer thin; isolate pure logic in independently testable JS functions. Use a Node test runner for local pure function tests; provide test commands and documented limitations of local mocks versus real Google services.
- Never copy formulas from the existing Excel tools blindly; verify references and run benchmark test cases. Do not claim live integration with external Excel workbooks until proven.
- No user-provided passwords, service tokens, signing secrets, test customer data or Google access credentials in source code, test fixtures, commits, logs or PR descriptions.
- Keep scope focused on MVP: do not implement payroll, BIM, AI, full inventory/ERP or payment gateway without explicit approval.

## Definition of done for each feature

A feature is done only when it has a working UI flow, server-side permission checks, input validation, Sheets schema/storage mapping, error/retry handling, happy-path and failure-path tests, and documentation. Distinguish code completed from deployed or user-tested functionality.
