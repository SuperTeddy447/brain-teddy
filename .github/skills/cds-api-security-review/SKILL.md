---
name: cds-api-security-review
description: Review the implemented CDS withdrawal, history and receipt APIs against OWASP API Security Top 10 with attack-focused evidence. Use during security verification and judge Q&A preparation.
---

# CDS API security review

Read the current OpenAPI contract, `docs/decisions.md`, `docs/security-defense.md` and `docs/test-matrix.md`. Inspect actual routes/security config before assessing risk. Distinguish real Entra/DB2 integration from a fallback and do not claim OWASP certification or full coverage from a short review.

Prioritize CDS financial and cross-branch risks: object-level authorization on guessed IDs; access-token signature/issuer/audience/expiry and operation roles; mass assignment of branch/status/amount/audit fields; state/limit/retry/concurrency abuse; bounded history/payload; safe errors, secret/config and route inventory. Compare these with the OWASP API Security Top 10 categories in `docs/security-defense.md`. Mark irrelevant categories with a reason, not as Passed.

Use synthetic identities and records. Run the highest-risk negative tests first (T04, T05, T20, T21, T09–T11); add focused tests only when they meaningfully protect the implemented code. Record exact revision, command, result and remaining gaps in the test matrix. Give BE a defect list ordered by money/authorization impact; SA owns the evidence and Q&A. Keep the first pass inside the 14:30–15:00 verification window.
