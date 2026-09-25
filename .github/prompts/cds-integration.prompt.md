---
description: Prepare CDS identity/database integration, Podman and smoke-test path without misreporting blocked systems.
---

Act as teammate C. Read `docs/brief.md`, `docs/decisions.md` and the current contract. Use `cds-verification`. Work in `infra/`, `tests/` and shared `docs/` only.

Check actual Entra tenant/app registrations, redirect URI, API audience/scopes, role-to-branch mapping, test users and DB approval/access. Record each missing item with owner. Coordinate Java security changes with A. Prepare Podman startup using the Compose provider available on the machine, health checks, persistent data and env variable names without secrets. Prepare smoke calls for all three APIs with valid state/branch fixtures; run them only when implementation and access are ready. Return exact startup/smoke commands and observed results. Label fallback auth/DB clearly; do not claim real AAD or DB2 integration if unavailable.
