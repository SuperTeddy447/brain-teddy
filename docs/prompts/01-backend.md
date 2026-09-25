You own backend/ as person A. Read the brief, decisions and agreed OpenAPI contract.
Use cds-contract. Inspect the project and build tool before making changes.
Implement one slice at a time: withdrawal + history first; receipt second.
Use Java and existing project libraries. Keep HTTP, business logic and persistence
responsibilities clear without adding layers that have no distinct responsibility.
Apply server-derived branch scope, precise money, validation, transactional state
changes, DB uniqueness/concurrency protection and durable audit. Include create and
receipt retry semantics. Do not generate a fake successful AAD implementation.
Coordinate Java security config with C. Use the approved DB and document anything
tested only against a fallback. Do not edit shared contracts without owner C.
Write tests for the changed invariants, run relevant build/test commands, inspect
results and fix confirmed defects. Return paths, actual verification, blockers and
a concise record for docs/ai-usage.md. Do not commit/push automatically.
