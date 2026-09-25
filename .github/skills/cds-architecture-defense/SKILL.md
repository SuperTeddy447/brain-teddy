---
name: cds-architecture-defense
description: Document and defend a minimal CDS modernization architecture with evidence-led scaling choices. Use when creating the architecture diagram, ADR or demo Q&A.
---

# CDS architecture defense

Start from the implemented or planned three-API scope: React client, Java business API, approved database and Entra ID boundary. Show real and mocked/blocked integrations distinctly. A modular Java backend is the default proposal until measured bottlenecks justify more services.

Explain why business state, amount and branch authorization live on the server; identify transaction/audit boundaries and how receipt confirmation stays singular under retry and concurrency. Match the diagram to the actual source and deployment. Do not use labels such as production-ready, scalable or AAD-integrated without supporting evidence.

For performance questions, separate total registered users from concurrent load. Show JMeter workload, p95/errors, app/DB placement and bottleneck observations. Consider pagination, query/index design and connection pool before adding topology. Discuss load balancer, cache, queue, replica or sharding only with a concrete trigger, benefit and consistency cost; cached limit/receipt state can become stale.

Deliver a compact Mermaid diagram in `docs/architecture.md`, one or two ADRs using `docs/decisions.md` format, and a 60-second explanation aligned with the demo.
