# Proposed Sheets storage model (v1)

Store each entity in a dedicated named sheet with a stable header row and schemaVersion. Add indexes/caches only after observing quotas and performance. Use `companyId` per installation for audit, and `projectId` on project-specific records. Do not let the caller set these values outside server-validated scope. `recordVersion` supports optimistic concurrency checks.

| Sheet | Key fields |
|---|---|
| `Settings` | key, value, updatedAt |
| `Users` | userId, identitySubject/email (verified), displayName, active, createdAt |
| `RoleAssignments` | roleAssignmentId, userId, role, projectId nullable, active |
| `Projects` | projectId, projectCode, name, client, contractValueIdr, startDate, endDate, status, pmUserId, createdAt, updatedAt, recordVersion |
| `BudgetLines` | budgetLineId, projectId, baselineVersion, wbsCode, description, budgetAmountIdr, approvedBy, approvedAt |
| `DailyReports` | reportId, projectId, reportingDate, authorUserId, activities, workforceNotes, progressNotes, reviewStatus, createdAt |
| `ExpenseRequests` | requestId, projectId, budgetLineId, title, description, amountIdr, requesterUserId, state, currentApprover, createdAt, recordVersion |
| `RequestItems` | requestItemId, requestId, description, qty, unit, unitPriceIdr, lineAmountIdr |
| `ApprovalEvents` | eventId, requestId, fromState, toState, actorUserId, reason, timestamp |
| `PurchaseEvents` | purchaseEventId, requestId, eventType, amountIdr, actorUserId, evidenceId, timestamp |
| `ExpensePayments` | expensePaymentId, requestId, projectId, amountIdr, paidAt, payee, evidenceId, actorUserId, externalRef, createdAt |
| `Termins` | terminId, projectId, sequence, amountIdr, plannedDate, invoiceDate, dueDate, notes, recordVersion |
| `TerminReceipts` | receiptId, terminId, projectId, receivedAmountIdr, receivedAt, evidenceId, externalRef, actorUserId |
| `Issues` | issueId, projectId, title, description, severity, status, assigneeId, dueDate, createdAt, closedAt, recordVersion |
| `IssueEvents` | eventId, issueId, actorUserId, fromState, toState, comment, evidenceId, timestamp |
| `Attachments` | attachmentId, projectId, relatedEntity, relatedId, driveFileId, fileName, mimeType, bytes, uploaderId, createdAt |
| `AuditLog` | auditId, actorUserId, action, entityType, entityId, projectId, timestamp, oldHash, newHash, detailsSafe |
| `Idempotency` | idempotencyKey, actorUserId, operation, resultReference, createdAt |

## Accounting semantics

- `budget` = approved baseline, not cash.
- `requested` = awaiting approval, not committed or paid.
- `approved` = authorized request, may be included in forecast commitments but not actual cash expense.
- `ordered` = purchase commitment, not confirmed receipt.
- `received` = goods/services recorded received, not paid.
- `paid actual` = sum of `ExpensePayments`; partial payments allowed.
- `invoice outstanding` = invoiced termins minus receipts. `cash received` = sum of `TerminReceipts`.
- Do not double-count a request when moving across lifecycle states.
- All amounts are IDR and ideally stored as integers (round currency consistently). Currency display `id-ID`.

## Referential integrity

Validate projectId exists and is permitted; budgetLineId belongs to project; terminId belongs to project; payment cannot exceed authorized amount without recorded exception workflow; receipt cannot exceed outstanding termin without explicit handling; editing baseline requires versioned review. Critical multirow writes require LockService and recovery instructions for partial failures.
