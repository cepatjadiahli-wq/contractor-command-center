# Product requirements — v1.0 MVP

## Mission and customers

**CONTRACTOR COMMAND CENTER** helps small and medium Indonesian contractor owners review field activity, approve material/expense requests, monitor spending vs budget, track payment terms, and close project issues with accountability. Primary buyer is contractor owner/director managing multiple simultaneous projects. Secondary users: PM, site engineer, finance, admin.

The app accompanies the 6 pillar / 3 guidebook / 120 SOP / 120 template / Excel calculator product, but it must be useful standalone and must not present the archive as automatically integrated. Documents are reference content. Functional control is the value proposition.

## Non-goals

Not a full ERP, accounting solution, legal compliance verifier, tender submission system, project scheduler, payroll, BIM, or mandatory subscription SaaS. No implied guarantee of profits, tender awards, statutory compliance, or return on investment.

## Functional scope

### M1 — Executive dashboard
- Select project / all permitted projects; read-only KPI cards.
- Show approved budget baseline, committed purchase amount, paid actual cost, invoiced/unpaid termins, cash received, and count of open issues/requests.
- Every KPI must have a drill-down / transaction source and clearly labeled units.
- Avoid presenting project profit as actual realized profit when project progress, costs and revenue recognition are insufficient. Mark forecasts as estimates.

### M2 — Project registry and budgets
- Create project: code, name, client, contract value, start/end, status, manager.
- Budget baseline by WBS/category imported from validated template CSV/Sheet. Preserve baseline version and who approved it.
- Read/edit permissions; prevent cross-project access.

### M3 — Daily site reporting
- Submit report: project, date, author, activities, progress notes, workforce notes, issues, photo attachments.
- Supervisor can review. File references are stored; do not expose Drive file publicly.
- Define progress as descriptive initially; do not fake earned-value calculations.

### M4 — Material and expense approvals
- Site/PM submits request: project, WBS code, item, quantity, unit, unit price, reason, supporting file(s).
- State machine: DRAFT -> SUBMITTED -> {REVISION_REQUESTED, REJECTED, APPROVED}. Resubmission after revision retains full history.
- If approved: ORDERED -> RECEIVED -> PAID, with different users/permissions and documentary evidence. Support partial payment as ledger entries; do not equate approval with cash payment.
- Approver sees budget availability and relevant commitments. Flag but do not auto-hide over-budget requests; configure block/warn policy in admin settings.

### M5 — Budget and cashflow
- Budget per WBS, approved commitments, receiving/fulfillment, paid cost, remaining budget as distinct metrics.
- Termins: scheduled, invoiced, due, partially received, received; amount/status history and outstanding balance.
- Project cash position = recorded cash receipts - recorded cash disbursements, with clear scope/date; not an accounting bank balance.
- Import historical costs via explicit validated import later; MVP focuses on transactions created within the app.

### M6 — Issue and report center
- Issue: title, description, severity, owner, due date, project, photos/evidence, history, status OPEN / IN_PROGRESS / RESOLVED / CLOSED / REOPENED.
- Resolution never auto-closes issue: explicit reviewer decision, with reason, time and identity.
- Export filtered summary/report as PDF or CSV where supported; never leak inaccessible projects.

## UX

Mobile-first daily-report and request forms. Desktop owner dashboard. Navbar: Ringkasan, Proyek, Laporan Harian, Permintaan, Anggaran & Cashflow, Masalah, Laporan, Pengaturan. Search/filter/pagination, loading/error/empty states; obvious action and confirmation for irreversible steps. All production labels Indonesian.

## Packaging

The application is installed in a private customer-specific Google environment (not a centralized SaaS). The seller distributes source/installable package plus clear configuration docs and licensing terms. Customer data stays within customer-owned Sheets/Drive as configured. Design for no hidden subscription fees, while explaining third-party quota or account limitations.
