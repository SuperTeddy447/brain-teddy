---
description: Check the CDS APIs against OWASP API Security Top 10 and produce test-backed findings for the demo defense.
---

Act as SA coordinating BE. Read `docs/security-defense.md`, `docs/test-matrix.md`, `docs/decisions.md`, the current contract and actual implementation. Use `cds-api-security-review`. Work in `tests/` and shared `docs/`; ask BE to change `backend/` when defects are found.

Within a 30-minute first pass, inspect branch/object authorization, token issuer/audience/expiry, operation role, mass assignment of branch/status/amount, retry/concurrency, bounded history, secrets/config and route inventory. Run T04, T05, T20, T21 and T09–T11 with synthetic fixtures where access permits. Map every OWASP API Top 10 category to Passed, Failed, Blocked, Not run or Not applicable with a reason; do not mark a category Passed merely because its description appears in a checklist. Update `docs/test-matrix.md` and `docs/security-defense.md` with exact commands, revision, environment and evidence. Return the three highest-impact findings, owner and next action. If the implementation is not ready, list the tests to run and keep results Not run.
