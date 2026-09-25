# Verification matrix

All rows start Not run. Fill actual revision, command and evidence after execution.

| ID | Scenario | Required assertion | Result / evidence |
|---|---|---|---|
| T01 | Valid withdrawal | One stored request; exact amount; correct branch and audit | Not run |
| T02 | Invalid amount/denomination/date | Contract error; no business mutation | Not run |
| T03 | Business limit boundary | Rule exactly as confirmed; no excess aggregate reservation | Not run |
| T04 | Missing/invalid/wrong-audience token | API rejects; no data leak or mutation | Not run |
| T05 | Cross-branch list/detail/write | Scope enforced even with a guessed resource ID | Not run |
| T06 | History filters/pagination | Correct scope/order/page bound; stable tie-breaker | Not run |
| T07 | Valid receipt | One allowed transition and consistent audit | Not run |
| T08 | Receipt wrong state/mismatch | Contract error; unchanged business state | Not run |
| T09 | Same-key same-payload retry | Same logical result; no duplicate effect | Not run |
| T10 | Same-key changed payload | Conflict; no new effect | Not run |
| T11 | Concurrent receipt | Single financial transition; defined responses | Not run |
| T12 | Concurrent withdrawal at limit | Aggregate invariant maintained if such limit exists | Not run |
| T13 | DB failure/rollback | No partial request/receipt/business audit | Not run |
| T14 | Response lost after commit | Retry/reconcile without duplicate effect | Not run |
| T15 | Restart/reload | Data persists and user can recover workflow | Not run |
| T16 | UI states/keyboard/narrow layout | Task can be completed; errors and pending states clear | Not run |
| T17 | Container clean start | Documented setup works on declared environment | Not run |
| T18 | Load test | Measured latency/errors/throughput with workload/environment | Not run |
| T19 | Identity/scope change | No cached data from previous user/scope displayed | Not run |
