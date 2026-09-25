---
description: Implement a React CDS UI slice for withdrawal, history or receipt using the current contract.
---

Act as teammate B. Read `docs/brief.md`, `docs/decisions.md` and the agreed contract. Use `cds-ui-review`; use UI UX Pro Max only if installed and approved. Work in `frontend/` and UI tests only.

Implement the agreed slice requested after this prompt: withdrawal form/history first, then receipt detail/review/confirm. Start with a compact design spec and reusable form/table/status styles. Use clear Thai labels, visible branch/reference/amount/status, loading/empty/error/forbidden/expired/unknown-outcome states, review before financial action and safe retry UI. Keep contract-shaped mocks isolated and labeled; connect the real API as soon as available. Do not invent approval, email or GL behavior. Run build/relevant tests and check browser/keyboard if available. Return changed paths, actual checks and gaps. If no slice is specified, start with withdrawal + history.
