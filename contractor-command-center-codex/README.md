# CONTRACTOR COMMAND CENTER — Codex Kickoff

**Product:** A mobile-first Google Apps Script Web App for Indonesian small/medium construction contractors, as the live operational component of THE SYSTEMIZED CONTRACTOR REVOLUTION.

## Use this package with Codex

1. Create a **private** GitHub repository named `contractor-command-center`.
2. Commit the files in this starter folder to that repository.
3. Open Codex (cloud or local), select the repository, and submit the task in `CODEX_START_PROMPT.md`.
4. Review Codex's plan and implemented code. Do not deploy to client data without testing the acceptance flows and security checks.
5. Set up Apps Script / `clasp` credentials **only in your own environment**. Do not commit OAuth tokens, `.clasprc.json`, production spreadsheet IDs, or private customer data.

**Important:** This package is a product brief and agent context—not an implemented, deployed, or security-audited application.

## Implementation order

- P0 Architecture, role/authn decision, data model, repository and test harness
- P1 Project registry + budget import + single-project basic dashboard
- P2 Site reports + attachments + issue tracking
- P3 Requests / approvals / fulfillment / payment and cashflow
- P4 Hardening, UX, installer, documented deployment and buyer onboarding

## References

- Google Apps Script HTML Service: https://developers.google.com/apps-script/guides/html
- Google Apps Script web app deployment: https://developers.google.com/apps-script/guides/web
- clasp: https://developers.google.com/apps-script/guides/clasp
