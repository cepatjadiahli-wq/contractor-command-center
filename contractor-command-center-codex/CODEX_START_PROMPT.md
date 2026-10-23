# Paste this as the first task in Codex

You are the lead software engineer for CONTRACTOR COMMAND CENTER, a commercial Google Apps Script web app for small/medium contractors in Indonesia.

Read AGENTS.md and all documents under docs/ before writing implementation code. Implement a safe, testable vertical slice for **Milestone 0 + Milestone 1**, not the entire ERP in one pass.

Deliver this task:

1. Assess the requirements and write `docs/DECISIONS.md` covering: secure identity for users with personal Gmail vs Workspace accounts, private per-company installation, role/project authorization, Apps Script quotas, how to separate code for Node local tests vs GAS runtime, and a realistic test strategy.
2. Bootstrap a Git-ready `clasp` Apps Script project with clean structure and development README. Do not invent a deployed URL or commit credentials.
3. Implement foundational server services: schema management, data-access wrapper for Sheets, project CRUD with authorization, budget baseline import/validation, audit logging, idempotency/locking for writes.
4. Implement HTML Service frontend with a professional responsive Indonesian interface: dashboard shell, project list/create form, project detail and budget overview. Use only real server data; supply an explicit demo seeding script for local/test installations.
5. Add meaningful pure-logic tests (permissions, validation, money accounting, state transition preparation) and documented manual test steps for GAS identity and services.
6. Add deployment instructions using clasp, including Google Apps Script API setup, least-privilege sharing, customer-owned Sheet/Drive resources, safe development vs production setup, and known limitations.
7. Summarize changed files, commands run, test results and remaining blockers at the end of the task.

Never claim that authentication, multiuser access, or deployment is proven until actually tested in Google. If reliable identity requires a Google OAuth setup, propose and document a secure option; do not substitute a user-selectable role/email dropdown or remove access checks. Avoid implementing customer login by password.

Keep functions modular and the interface professional: concise navigation, readable KPI cards, small-screen layout, accessible form labels, table filters, error/loading/empty states. Monetary amounts must be accurate and formatted for IDR. No third-party paid dependencies.

Do not implement daily reporting, approvals, termins and issue lifecycle in this *first* task, except for interfaces/types needed to support follow-on milestones. Finish the vertical slice, tests and docs instead of stubbing six unfinished modules.
