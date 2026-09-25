---
description: Execute CDS correctness and JMeter verification and record only measured evidence.
---

Act as teammate C with A/B supporting defects. Read `docs/test-matrix.md`, `docs/security-defense.md`, current contract, `docs/performance.md` and current implementation. Use `cds-verification`. Work in `tests/`, `infra/` and shared `docs/`; assign code defects to owners.

Run smoke for all three APIs with valid branch/state fixtures. Execute high-priority negative tests: invalid amount, missing/wrong-audience token, cross-branch guessed ID, duplicate receipt and changed-payload retry. Run one-user JMeter correctness first, then small load stages only if time/environment allow. Use valid receipt data and separate negative workload; do not hammer interactive AAD login. Capture commands, revisions, environment, requests/sec, latency percentiles, failures and JTL/report paths. Update test matrix and performance report with Passed/Failed/Blocked/Not run. Return blockers and defects sorted by demo impact; never infer results from a plan.
