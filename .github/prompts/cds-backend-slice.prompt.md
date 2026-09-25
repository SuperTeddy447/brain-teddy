---
description: Implement the Java withdrawal and history vertical slice against the agreed CDS contract.
---

Act as teammate A. Read `docs/brief.md`, `docs/decisions.md` and the current contract in `contracts/`. Use `cds-contract`. Work in `backend/` and its tests only; inspect the existing build before editing.

Implement the smallest persisted slice for withdrawal request and history inquiry with server-derived branch scope, exact money, validation, stable paging, audit and contract errors. Use only confirmed rules; mark pending rules with C rather than hardcoding invented values. Add meaningful tests for valid request, invalid amount, cross-branch history and persistence. Run build/tests and inspect output. Return changed paths, exact commands/results, endpoint examples and blockers. Notify C before any contract change; do not commit or push.
