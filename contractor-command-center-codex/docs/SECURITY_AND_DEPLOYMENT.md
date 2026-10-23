# Security, deployment, commercial distribution

## Authentication and authorization: unresolved architectural gate

Apps Script can run Web Apps as the script owner or as the accessing user. The availability of `Session.getActiveUser().getEmail()` varies. Codex must investigate the actual deployment access mode and provide **a tested identity and role plan** before implementing staff-facing sensitive endpoints. A UI login selector or client-provided email/role is never sufficient. If owner-only pilot is needed, document this limitation instead of shipping insecure multiuser access.

Choose default-deny authorization, server validated Google identity, role+project permissions, and a secure private-company installation. Avoid anonymous access to internal financial information. If a proposed identity mechanism requires a separate Google Cloud OAuth configuration or workspace domain, disclose and document it. No unaudited token handling.

## Permission matrix (provisional)

| Activity | OWNER | ADMIN | PM | SITE | FINANCE |
|---|---|---|---|---|---|
| Project create/configure | Yes | Yes | Read | Read scoped | Read scoped |
| Budget baseline approve | Yes | Limited configured | View | View scoped | Propose |
| Daily report submit | Yes | Yes | Yes | Yes scoped | View |
| Request submit | Yes | Yes | Yes | Yes scoped | Yes |
| Request approve | Yes | Delegated configured | Limited configured | No | No |
| Record order/receipt | Yes | Yes | Yes scoped | Limited scoped | View |
| Record payments | Yes | No by default | No | No | Yes |
| Termins/receipt record | Yes | Read | Read | No | Yes |
| Issue close | Yes | Configured | Yes scoped | No (resolve only) | Configured read |
| Manage users/roles | Yes | Configured | No | No | No |

Implement permission checks in server code on every read/write; do not merely hide UI buttons. Test unauthorized project access and role escalation.

## Installation/release strategy

- One private Apps Script project + spreadsheet + Drive folder per company; not multi-tenant shared-sheet hosting.
- Maintain source in a private GitHub repo; use `@google/clasp` to push to a dedicated development project, then separate production project after tests.
- Installer/setup instructions: create database Spreadsheet and Drive root folder, set Script Properties / IDs, bootstrap first owner identity safely, seed demo data only in test environments, check permissions, publish restricted Web App and record URL.
- Never distribute seller-owned `.clasprc.json` or OAuth tokens. Add `.clasp.json` to local ignore if it contains customer-specific script IDs; protect credential files.
- Deployment can be delayed until a manual security review. Provide end-user permission/OAuth screenshots guidance but do not claim one-click setup prematurely.
- Write runbook for backup/export, app upgrade, error recovery, versioned migrations, and quota troubleshooting.

## GAS practical limits

Short execution duration, concurrent-request limits, Sheets quotas and Drive file/permission constraints. Batch reads/writes, cache read-only metadata safely, paginate, avoid treating Sheets as an unbounded relational database, and document realistic pilot limits derived from tests rather than invented capacity claims.
