---
name: cds-legacy-modernization
description: Map the CDS ASP Classic/VB6/DB2 legacy flows to the three new APIs and prepare an evidence-backed incremental migration explanation. Use during briefing and architecture or demo defense, not for speculative implementation.
---

# CDS legacy modernization

Read `docs/brief.md`, `docs/decisions.md`, `docs/requirements.md` and `docs/legacy-modernization.md`. Confirm the actual legacy components and data ownership with the organizer. Separate **implemented**, **fixture/fallback**, **blocked** and **future** in every diagram and demo claim.

Map only the three required business operations first: legacy screen/function → new UI/API → datastore and identity boundary → deferred external integrations. Identify who owns the source of truth for a request, where branch identity comes from, and how old/new references, money, timezone and status would map. Mark unknown DB2 schema and data migration steps as pending; do not invent adapters, dual-write or cutover.

Recommend an incremental route: release one tested slice, preserve remaining legacy behavior, then plan reconciliation, rollback and later function migration after the hackathon. No proxy, service split, cache or queue is required merely to claim modernization. Use measured bottlenecks and consistency needs before proposing them.

Deliver a one-page migration map and a 60-second defense aligned with the actual implementation. Budget at most 15 minutes during the 16:00–17:00 review block; if the three API demo is unstable, leave the map as a truthful outline and spend the time on the blocker.
