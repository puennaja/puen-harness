# Phase 1 — Review Decisions and Risks

> Documentation approved by the user on 2026-10-17. The user confirmed every recommendation in Q1–Q34 and approved the assembled documents, schema-key spellings, command representation and test IDs. No implementation or live provider testing is authorized by this record.

## Review outcome

The initial Phase 1 draft was not yet testable: an approved-write success path, Ollama execution ownership, effective permission/billing boundaries, routing eligibility, Task/Run/session identity, context authority and failure states were undefined. Minimal policy, safety recovery and journaling also depended on later phases.

The approved [Phase 1 contract](phase-01-foundation.md) and [A01–A40 acceptance matrix](phase-01-acceptance.md) resolve those product decisions. The residual unknowns below require measured provider qualification; they are not permission to guess or relax the contract. Phase 2/3 have not undergone their own full review.

## Confirmed discussion decisions

This is the historical decision ledger. Current behavior is governed by the approved phase specification rather than by independently maintained copies of these summaries.

| Question | Confirmed recommendation | Acceptance coverage |
| --- | --- | --- |
| Q1 | A small approved local coding task with diff/check results; no commit/push/PR/deploy. | A13, A36, A39 |
| Q2 | Ubuntu and macOS targets. | Qualification matrix |
| Q3 | Ollama text-only; no harness-owned coding loop or filesystem/shell tools. | A11, A20 |
| Q4 | Tested provider-native enforcement is allowed; unknown enforcement fails closed. | A12, A37, A39 |
| Q5 | Monetary budgets are admission control, not guaranteed final financial ceilings. | A24–A26 |
| Q6 | Reviewed task-capability table and bounded qualification fixtures; unknown task class needs manual choice. | A05–A07 |
| Q7 | Approved project data policy before remote requests, including routing/classification. | A08–A11 |
| Q8 | Fixed Run approval envelope; scope expansion/dependencies/outside access need new authorization. | A12, A15, A39 |
| Q9 | Trusted external user-policy ceiling and approved project snapshot; repository cannot expand permissions. | A08–A10 |
| Q10 | Durable Task, bounded provider Run, separate Native Session; one active Run per instance. | A17, A23, A32 |
| Q11 | Private state outside repo; metadata journal, no default raw transcript; explicit deletion; no automatic expiry in MVP. | A33–A35 |
| Q12 | No harness automatic retry/fallback; distinguish failure, cancellation and unknown completion. | A27–A31 |
| Q13 | Unknown-cost permission is project/profile specific and invalidated by billing changes. | A09, A25 |
| Q14 | Manual/static Auto; local-only is a hard constraint; defer cost-first/quality-first. | A05–A07, A11 |
| Q15 | Ubuntu 24.04 LTS x86_64 and macOS 15 Apple Silicon qualification targets. | Qualification matrix |
| Q16 | Three adapter baselines; both first-party subscription/API coding profiles qualify separately; same coding adapter supports edits on both OS targets. | Qualification matrix, A13 |
| Q17 | Dedicated qualified configuration; no unapproved executable extensions, memories or child/background agents. | A16, A34, A37 |
| Q18 | Separate requested/reported model; stop on missing identity/mismatch/reroute. | A03–A04 |
| Q19 | User-approved requirements revisions; canonical JSON and generated Markdown. | A18–A21 |
| Q20 | Writes require clean Git workspace; no automatic stash/reset/cleanup. | A13–A14 |
| Q21 | Fresh Native Session per Run; native resume and same-Task route changes deferred. | A17 |
| Q22 | Persistent per-Task admission/usage, no failed-attempt refund or zero for unknown usage. | A23–A26 |
| Q23 | Reject oversized prepared context; no silent truncation or harness compaction. | A19–A22 |
| Q24 | Versioned task-specific static provider/model priorities and explicit tie-breaking. | A05–A07 |
| Q25 | Run completion, check result and human Task acceptance remain distinct. | A36 |
| Q26 | Explicit Task commands/approval; active Runs reject new work/mutations; default full history is transient. | A02, A17, A35 |
| Q27 | Scope expansion denies/ends current Run; a new Run needs fresh approval. | A15 |
| Q28 | Required source/schema/workspace mismatch blocks; optional unused stale material is labelled. | A19, A40 |
| Q29 | Local deterministic preparation and explicit file selection; no hidden model calls. | A06, A20 |
| Q30 | Defaults: 3 Runs/Task, 15 minutes/Run; cancellation 5 seconds graceful + 5 seconds forced cleanup. | A23, A27–A28 |
| Q31 | Durable admission before invocation; stop on persistence failure; locks/crash reconciliation without replay. | A30–A32 |
| Q32 | Disclose native artifacts; no-transcript preference; raw persistence opt-in; deletion owns only harness data. | A33–A35 |
| Q33 | Workspace-wide data authorization; provider API traffic is separate from tool network; no all-secret detection claim. | A10–A12, A39 |
| Q34 | Contract tests plus actual provider/platform enforcement evidence; mocks cannot certify safety. | All cases and release sign-off |

## Ambiguities and contradictions addressed

| Original gap | Approved resolution |
| --- | --- |
| Only unsafe-write rejection demonstrated; no approved-write success case. | Explicit small-edit demo and independent completion/check/acceptance states. |
| Ollama treated as if it necessarily owned a coding-agent loop. | Text-only adapter; native coding loops belong to Codex/Claude. |
| Policy precedence/configuration deferred to Phase 3 despite Phase 1 safety dependence. | F17a minimal policy in Phase 1; F17b richer UX in Phase 3. |
| Failure/recovery assigned wholly to Phase 2. | F13a stop-and-report now; explicit fallback/recovery review later. |
| Full journal deferred despite Phase 1 audit requirements. | F12a private metadata journal now; richer approved evidence/export in Phase 2. |
| Quality floor without initial evidence; provider-first order without pair aggregation. | Bounded task fixtures, pair filtering, provider eligibility iff a model survives, then static provider/model priorities. |
| Read-only, adequate approval and unsafe write undefined. | Fixed envelope, actual negative enforcement tests, strict scope-expansion lifecycle. |
| Unknown cost and incomplete usage could appear free or regain allowance. | Profile-specific opt-in, persistent uncertainty/reservations and no automatic refunds. |
| Task pack authority, revision and restart behavior undefined. | Canonical versioned JSON; user-approved revisions; fresh native sessions; explicit route binding. |
| Sensitive-data flags/freshness omitted during phase extraction. | Required source/data-policy/context metadata and workspace-wide authorization. |
| Native persistence conflated with harness journal and deletion. | Explicit ownership/footprint/consent and restricted deletion guarantee. |
| Exit gate conflated requirements approval with post-implementation evidence. | Separate documentation approval, implementation authorization and MVP acceptance. |

## Phase changes

| Phase | Change |
| --- | --- |
| Phase 1 | Include minimum F12a journal, F13a stop-and-report, F17a policy and all Task Context v0 validation. This is foundational safety, not advanced orchestration. |
| Phase 2 | Own native resume, same-Task provider/model continuation, semantic/scoped retrieval, compaction, cross-provider handoff, workflow/skill integration, independent verification, richer evidence and explicit recovery/fallback. |
| Phase 3 | Own comparative/weighted/learned routing, cost-first/quality-first strategies, verified learning memory and expanded configuration UX. |
| Post-MVP, placement not yet approved | Cross-Task daily/global caps, automatic retention jobs, additional OS/architectures and broader auth/gateway integrations. Phase 3 must decide whether to include the first two during its review. |

The historical consolidated backlog keeps its original body; its navigation note marks old MVP scenarios/routing modes as historical. No claims are made about `puen-engineer` or `puen-stack` internals; those repositories were not inspected.

## Residual risk register

| Risk | Impact | Required control/evidence | Residual limitation |
| --- | --- | --- | --- |
| R1 Native permissions are coarser than the approved envelope. | Unapproved file/shell/network effects or no usable coding route. | Per-profile live negative tests; strict sandbox/bypass/extension configuration; fail closed. | The agreed contract may be infeasible on a chosen CLI/profile. Return to the human; do not weaken it silently. |
| R2 Auth/profile isolation differs by mode. | Subscription configuration inherits unsafe extensions or an assumed clean mode cannot authenticate. | Qualify first-party subscription and API separately, including effective configuration and helpers. | Claude bare mode is API-oriented and cannot simply be assumed to reuse subscription login; an alternative safe subscription configuration needs proof. |
| R3 Model alias/substitution/reroute. | Wrong model/cost/capability and invalid pin claims. | Resolve qualified IDs, observe identity events, stop on mismatch, record both identities. | Provider-reported identity is not cryptographic proof of backend weights; a detected reroute cannot undo prior processing. |
| R4 Ollama local endpoint uses cloud inference. | Local-only data leaves the machine. | Verify running server's no-cloud configuration and qualified local model/digest; inspect actual egress behavior. | A child environment flag does not reconfigure an existing daemon; diagnostics alone are not certification. |
| R5 Context estimates/capacity differ by model, language and native tools. | Overflow, silent loss or misleading budget. | Explicit methods/reserves, Thai/Unicode fixtures, qualified capacity and overflow tests, immutable requirements. | Harness controls prepared payload only; native context behavior and model compliance are not fully observable or guaranteed. |
| R6 Estimates, subscriptions, retries and cancellations obscure cost. | Overrun or misleading totals. | Admission reservations, per-Task attempt/time caps, labelled unknowns/overruns and profile-specific unknown-cost permission. | No strict in-flight financial ceiling; no cross-Task/global ceiling; remote cancellation may not stop billing. |
| R7 Native logs/caches and provider retention exceed harness storage. | Sensitive content persists unexpectedly. | Qualified no-transcript modes, declared footprint/ownership, explicit raw opt-in and provider-policy links. | Harness deletion cannot erase shared/native/provider artifacts or backups; no zero-retention promise. |
| R8 Crash, disk failure and multiple processes leave uncertain work. | Duplicate writes, lost evidence or unsafe restart. | Durable admission, locks, stop-on-persistence-failure, operator review of unfinished Runs and PID identity. | Abrupt machine failure may leave unconfirmed remote activity or partial local writes. |
| R9 Workspace classification/command allowlists are mistaken for confinement. | Secrets or indirect script side effects escape. | Whole-accessible-workspace data policy, trusted runtime/credential boundary, path/symlink/network tests including allowed scripts. | No universal secret detection or malicious-provider defense; operator/OS/qualified runtime remain trusted. |
| R10 Qualification effort is large for an MVP. | Two OS targets and ten baseline rows delay usable release. | Preserve agreed minimum, reuse deterministic fixtures and schedule live profile tests explicitly. | Any reduction of mandatory rows needs a new human scope decision. Missing optional writes stay disabled. |
| R11 Static eligibility may not predict real task quality. | Incorrect output despite a qualified route. | Small fixtures with explicit scope, preserved evidence and user criterion acceptance; independent verification deferred. | No generalized quality guarantee or evidence of routing improvement in Phase 1. |
| R12 Official interfaces/settings evolve. | Discovery, permissions, persistence or normalization regress after upgrade. | Pin tested profile versions, structured interfaces and configuration fingerprints; invalidate unqualified changes. | Documentation facts are not executed conformance; implementation transport/runtime choices remain to be proposed under separate authorization. |

## Primary-source facts informing qualification

Read on 2026-10-17. Links are supporting documentation, not a substitute for measured evidence on the selected versions.

| Source | Bounded fact relevant to the spec |
| --- | --- |
| [Codex App Server](https://developers.openai.com/codex/app-server/) | Structured approval/model events exist, including reroute notifications; choose and qualify an interface instead of assuming identical CLI transports. |
| [Codex non-interactive mode](https://developers.openai.com/codex/noninteractive/) | JSON output and ephemeral-session support inform candidate adapter/persistence design; they do not establish a financial ceiling or eliminate every artifact. |
| [Codex sandboxing](https://learn.chatgpt.com/docs/sandboxing) | Native enforcement is platform dependent; Ubuntu and macOS need separate evidence. |
| [Claude headless](https://code.claude.com/docs/en/headless) | Headless permission hosting, lifecycle and bare-mode behavior differ. Bare mode must not be assumed compatible with subscription authentication. |
| [Claude CLI reference](https://code.claude.com/docs/en/cli-reference) | Tool approval and tool availability are distinct; budget/session-persistence flags do not prove invoice or complete-storage control. |
| [Claude sandboxing](https://code.claude.com/docs/en/sandboxing) | Bypass/retry/exclusion settings matter; enabling a sandbox is not by itself fail-closed evidence. |
| [Claude application data](https://code.claude.com/docs/en/claude-directory) | Multiple native artifact classes require separate footprint/cleanup review. |
| [Ollama FAQ](https://docs.ollama.com/faq), [Cloud](https://docs.ollama.com/cloud) | Local clients can use cloud models; disabling cloud features applies to the running server and requires restart after changes. |
| [Ollama model metadata](https://docs.ollama.com/api/tags), [generation](https://docs.ollama.com/api/generate), [context](https://docs.ollama.com/context-length) | Discovery/digests and usage signals are available, but effective context/resource and cancellation behavior still require tests. |
| [Claude data usage](https://code.claude.com/docs/en/data-usage), [OpenAI API data controls](https://developers.openai.com/api/docs/guides/your-data) | Service/account retention is separate from local transcript settings. Do not generalize one billing mode's policy to all subscriptions/API accounts. |

No exact provider versions, model quality ranks, monetary prices or completed conformance results are invented in this specification. Populate those release artifacts from explicitly authorized qualification before enabling the corresponding routes.

## Approved document changes

- Replace Phase 1 with the coherent behavior/safety/context/accounting contract.
- Add acceptance and review companions, a concise domain glossary and two ADRs for durable architectural choices.
- Update phase navigation and narrow Phase 2/3 inherited boundaries without claiming those phases are reviewed.
- Clarify the consolidated backlog's historical status without rewriting its original body.
- Keep existing specification language English; discussion and approval summaries remain Thai. No blanket translation or code changes are proposed.

The user approved applying this documentation package as one cohesive local commit. Implementation and live provider execution remain separately unauthorized.
