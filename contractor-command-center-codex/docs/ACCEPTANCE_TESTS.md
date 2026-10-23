# Acceptance tests and release gates

## Critical end-to-end scenario (dummy data only)

Given project P-001 with WBS line `MAT-01` budget Rp100,000,000:

1. Authorized owner creates project and enters WBS baseline. Verify unique IDs and valid data model.
2. Assigned SITE user submits daily report and one JPG. Only assigned project members can access it; invalid MIME/oversize rejected.
3. SITE requests Rp8,000,000 for materials tagged `MAT-01`. State becomes SUBMITTED; paid actual remains Rp0.
4. OWNER requests revision, SITE revises and resubmits, OWNER approves with audit trail. State APPROVED; cash paid remains Rp0.
5. ADMIN records ORDERED and RECEIVED (support distinct event dates, proofs). Ordered/received do **not** increase paid actual.
6. FINANCE records first payment Rp3,000,000 and later Rp5,000,000; paid actual becomes Rp8,000,000 exactly once (including after browser retry). Underlying ledger supports partial payments.
7. FINANCE sets termin invoice Rp50,000,000 and receipt Rp20,000,000; outstanding Rp30,000,000. Owner dashboard matches transactions.
8. SITE creates an issue with evidence and assignee; resolves it, but it stays nonclosed until authorized reviewer explicitly closes it. Reopen action retains event history.
9. Export transaction summary for P-001 and confirm no other project's data is present.
10. Unassigned user attempts to read/update P-001 using forged projectId/role and is denied by server, not merely by disabled UI.

## Security cases

- Blank/unknown identity does not grant access.
- Invalid state transitions rejected (e.g., SUBMITTED -> PAID).
- Client forgery of role, amount, approval actor, company/project scope is rejected.
- Duplicate submit with same idempotency key creates one record.
- Concurrent approvals do not double-commit.
- User input is safely displayed, sheet injection escaped, report generation escaped/sanitized.
- Invalid file types and oversized uploads rejected; customer documents never set public share-by-link by default.
- Missing/corrupt Sheet headers detected with migration warning, not silent data loss.

## Performance and UX cases

- Mobile viewport at 360px works for daily report and requests.
- Loading, empty and error states are visible.
- Monetary figures distinguish requested, committed, actual paid, invoiced, and cash received.
- Display totals reconcile to source data; refunds/reversals require separate event rather than hidden overwrite.
- Document quota/load-test limits, observed response times and support assumptions.

## Release gate

No public sale/deployment to live customers until authentication method and project isolation are validated on an actual Google Apps Script deployment using at least two separate Google accounts, tests pass, and a backup/recovery procedure is exercised.
