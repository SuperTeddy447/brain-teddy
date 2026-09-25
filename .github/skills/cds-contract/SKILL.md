---
name: cds-contract
description: Define or review the three CDS cash withdrawal and receipt APIs, including money, state, branch scope, error and retry semantics. Use during contract design or business-rule changes.
---

# CDS contract

Read `docs/brief.md`, `docs/decisions.md` and the organizer's API standard first. Ask for only questions that block correctness; assign each pending answer an owner. Record confirmed rules separately from reversible assumptions. Person C owns contract files and gets A/B agreement before changing them.

For each required operation specify request/response examples, validation, error shape, auth scheme, role/branch scope and persistent effect. Do not assume endpoint names, schema, amount limits, cutoff rules or mandatory lifecycle steps before the organizer confirms them.

Resolve these invariants explicitly:

- Money representation and rounding; no binary floating point for financial values.
- Timezone, business date and cutoff; state eligibility for receipt.
- Server-derived branch identity; cross-branch list/detail/write rejection.
- Stable history ordering and bounded pagination.
- Idempotency key lifetime/scope, same-key same-payload replay, changed-payload conflict, and response-lost-after-commit recovery.
- One financial transition under concurrent confirmation; database uniqueness/locking or equivalent protection.
- Audit fields and transaction boundary.

Deliver a proposed or confirmed OpenAPI contract plus a concise decision list in `docs/decisions.md`. Include one success and one rejected example per operation, and record what remains unknown. Keep planned Center/approval/dispatch behavior distinct from implemented APIs.
