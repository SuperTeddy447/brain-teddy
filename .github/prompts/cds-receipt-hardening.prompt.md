---
description: Implement and test receipt confirmation, retries and transaction safety in the Java CDS API.
---

Act as teammate A. Read the current contract and `docs/decisions.md`. Use `cds-contract` and `cds-verification`. Work in `backend/` and backend tests only.

Implement receipt confirmation only for confirmed eligible state. Enforce server branch scope, exact amount/payload rules, one atomic state transition, audit, idempotent same-key retry and changed-payload conflict per contract. Protect concurrent confirmation and rollback on database failure. If eligibility, partial receipt or mismatch policy is unresolved, ask C for organizer answer and keep behavior explicitly proposed. Add tests for success, wrong state, duplicate, cross-branch, changed payload, concurrent attempts and rollback where feasible. Run relevant tests; report actual results and remaining gaps. Do not commit/push or edit shared contract files.
