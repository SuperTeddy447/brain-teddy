---
name: cds-ui-review
description: Design or review the CDS React withdrawal, history and receipt flows for clear bank-operations forms, scope, error states and accessibility. Use for frontend screens and rehearsal.
---

# CDS UI review

Read the current contract, `docs/decisions.md` and the actual UI scope. Review the three task paths: create withdrawal, inspect history, confirm eligible receipt. Prioritize legible Thai labels, clear branch/reference/amount/status, review before financial submit and an explicit result after submit.

Check loading, empty, validation, forbidden, expired login, network error and unknown outcome after a timeout. Prevent accidental duplicate action in UI but retain server idempotency. On identity/scope change, clear scoped cached data. Keep mocks isolated and visibly labeled until real API integration.

Check keyboard order, focus after errors, labels, narrow viewport and readable table overflow. Verify by running build/UI tests and browser checks if tools are available. Report what was actually inspected and remaining issues; never report a visual or accessibility pass solely from reading source.

If UI UX Pro Max is available and approved, use it as a source of layout ideas for these flows, then review generated work against this skill and the contract. Its installation is optional; see `docs/copilot-setup.md`.
