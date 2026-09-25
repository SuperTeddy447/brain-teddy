Read docs/brief.md and docs/decisions.md plus the organizer's actual requirements.
Act as our technical lead for a three-person CDS team. Work only on contracts/
and shared docs/ as owner C. First reconcile any contradictions in scope/rules.
Use cds-contract and cds-architecture-defense. Propose a small modular Java backend,
React client, and the approved database. Identify AAD/DB access blockers immediately.
Write the three-operation OpenAPI contract using the organizer's standard, or label
our proposed format explicitly. Define money, time, state, pagination, scope and
idempotency semantics. Include success and failure examples, security scheme and
concurrency invariants. Do not invent mandatory approval or dispatch workflows.
List only the business questions that block correctness. Record reversible assumptions.
Create docs/architecture.md with a compact Mermaid diagram of actual planned scope,
marking external systems and unimplemented integrations. Prepare a testable first
slice and tasks for A/B/C. Do not write application code yet or invent test results.
