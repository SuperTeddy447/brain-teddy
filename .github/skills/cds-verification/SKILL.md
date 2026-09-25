---
name: cds-verification
description: Verify the CDS three-API implementation with correctness, authorization, retry, restart and measured JMeter evidence. Use at integration, reliability and submission gates.
---

# CDS verification

Use `docs/test-matrix.md` as the scenario list. Read the current contract and implementation; mark each row Passed, Failed, Blocked or Not run with revision, command, environment and evidence. Do not turn planned tests into claimed results.

Prioritize exact money/limit behavior, rejected cross-branch access even with guessed IDs, invalid token/audience, eligible receipt, duplicate same-key replay, changed-payload conflict, concurrent receipt, rollback and restart persistence. Confirm a rejected operation causes no business mutation or partial audit.

For JMeter, start with one-user correctness, then small staged load if time and environment permit. Use realistic branch distribution and a workload mix that keeps receipt test data valid; separate negative tests from load. Reuse valid API tokens appropriately; do not load-test interactive AAD login. Record setup, virtual users, achieved throughput, latency percentiles, error categories, JTL/report paths and limitations in `docs/performance.md`.

Run the documented Podman clean-start and all three API smoke calls before claiming demo readiness. Give C concise evidence for submission; disclose unavailable AAD/DB access and any fallback used.
