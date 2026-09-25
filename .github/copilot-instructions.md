# CDS repository instructions for GitHub Copilot

Read `docs/brief.md`, `docs/decisions.md` and the current `contracts/` OpenAPI before implementing. The organizer's confirmed rules override preparation assumptions. Label any unconfirmed contract as proposed.

- Work only in the paths owned by the assigned teammate in `docs/cookbook.md`; ask owner C to coordinate shared contract changes. Do not rewrite another teammate's files.
- Build the three required operations first: withdrawal request, withdrawal history, cash receipt confirmation. Do not invent mandatory approval, dispatch, email or GL flows.
- Use Java for the business API and React for UI; Node is frontend tooling unless an approved requirement calls for more.
- Enforce token audience/issuer, role and branch scope on the API, including history/detail by guessed ID. Do not claim mocked authentication is AAD integration.
- Use precise money representation, atomic state transitions, idempotent request/receipt behavior and durable audit. Apply the organizer's actual business rules.
- Keep fixture/mock data labeled and synthetic. Do not put credentials, tokens or customer data in source, prompts, logs or test reports.
- Run relevant build/tests after edits. Report exact command, result, revision/paths and any untested behavior. Never invent coverage, performance or AI productivity metrics.
- Do not commit, push, submit or install third-party packages automatically unless the teammate explicitly requests it in the current task.

Use the targeted skills in `.github/skills/` when the task needs contract, UI review, verification or architecture defense. See `docs/copilot-setup.md` for invocation.
