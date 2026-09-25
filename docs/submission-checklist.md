# Submission checklist — due 18:00 Thailand time

- [ ] Organizer confirmed official deliverables and source freeze rule.
- [ ] Required three APIs match the agreed contract and operate on persisted data.
- [ ] Real AAD login/API authorization evidence, or prominently disclosed blocker.
- [ ] Required DB tested, or approved fallback and limitations disclosed.
- [ ] README has actual prerequisites, exact versions, environment names and run commands.
- [ ] Secrets excluded; no real token/test credentials in repo, screenshots or JMeter results.
- [ ] Architecture diagram matches implementation; planned/mocked integrations are labeled.
- [ ] OpenAPI and technical documentation match implementation.
- [ ] P0 requirements in `docs/requirements.md` have evidence or explicit Blocked/Not run status; P1 claims only what ran.
- [ ] Legacy-to-new map labels each integration implemented, fixture/fallback, blocked or future; data ownership is explained.
- [ ] Test matrix, measured coverage when required, load results and limitations attached.
- [ ] OWASP API Top 10 review in `docs/security-defense.md` cites tested evidence; unrun/blocked categories are not marked passed.
- [ ] AI usage summary includes prompts, human corrections and real evidence.
- [ ] Preparation and third-party licenses/sources are attributed as required.
- [ ] A teammate has run the documented clean-start path.
- [ ] Demo script has deterministic fixtures and shows success plus a rejected operation.
- [ ] Team can explain why a newly requested withdrawal may not be eligible for receipt yet; seeded eligible data is identified clearly.
- [ ] Backup recording uses synthetic data and excludes credentials.
- [ ] Reviewed source revision recorded; no unreviewed changes added after rehearsal.
- [ ] Repository access and submitted links/files checked from the actual submission.
- [ ] SharePoint upload completed before deadline, ideally by 17:50.

Demo outline: context (30s), login and branch (30s), withdrawal and history (90s),
eligible receipt (60s), duplicate/cross-branch rejection (45s), AI correction and
test evidence (60s), trade-off and limitation (45s). Adjust to the actual demo slot.
If approval/dispatch is not implemented, disclose that receipt uses a seeded
eligible record. Do not imply the full lifecycle was executed end to end.
