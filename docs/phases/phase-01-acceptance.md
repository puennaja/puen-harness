# Phase 1 — Acceptance Contract

> Documentation approved by the user on 2026-10-17 after Q1–Q34. This is a test specification, not executed evidence or authorization to invoke providers.

## Qualification matrix and evidence

Read [the Phase 1 specification](phase-01-foundation.md) for behavior and [the review record](phase-01-review.md) for decisions and risks.

Qualification targets are Ubuntu 24.04 LTS x86_64 and macOS 15 Apple Silicon. For **each** target require:
- Codex baseline with native subscription login and with first-party direct API.
- Claude Code baseline with native subscription login and with first-party direct API.
- Ollama text baseline with a local, cloud-disabled service and installed model.
- Approved-edit capability on at least one same coding adapter across both targets. Every enabled write profile needs its own evidence; another auth mode's result does not qualify it.

This is ten mandatory baseline rows across the two targets. The exact provider/model/interface versions are required fields of the measured conformance manifest, not invented version numbers in this specification. No row is currently certified.

A release row records: OS/architecture; provider/CLI/interface version; model ID/digest; auth/billing mode and secret-free endpoint/account reference; effective policy/configuration/extension fingerprints; capability flags; task-class fixture IDs; effective context/input/output allowances and estimation methods; native persistence footprint; cancellation behavior; evidence references and tester/date. Route metadata/priorities and reviewed fixtures must be versioned.

Status values are **not tested**, **passed**, **failed**, and **not applicable with reason**. Missing installation/auth in an operator's environment is a diagnostic state, not release qualification. A missing mandatory matrix row blocks MVP acceptance. An optional capability is disabled until passed; a human-approved change to mandatory scope is required to remove a mandatory row.

Evidence must distinguish:
- Deterministic harness/adapter tests, including fault injection for rare protocol failures.
- Live native-interface smoke tests and actual OS permission/filesystem/network/cleanup tests.
- Bounded task-class qualification fixtures; no generalized quality or benchmark claim.

Mocks cannot establish native permission, locality, persistence or platform conformance. A synthetic reroute or disk fault must be labelled synthetic; never claim to have observed that service behavior live. Use temporary workspaces, fake sensitive markers, approved test endpoints/accounts and explicit test spending permission. Preserve only evidence permitted by the tested persistence policy.

## Roles

**All** applies to harness behavior for every route. **Coding** applies to Codex/Claude baseline profiles. **Write** applies to enabled approved-edit profiles. **Local text** applies to Ollama. For a tool-less route, an unsupported tool action must be rejected before invocation; it does not require inventing native filesystem tools.

## Required scenarios

| ID | Role | Setup/action | Required observable result |
| --- | --- | --- | --- |
| A01 | All | Launch with absent provider binary, absent auth, unavailable Ollama or no qualified model. | Terminal stays usable; local diagnostics name the reason and manual recovery; no download, login, remote probe or fallback without authorization. |
| A02 | All | Exercise documented slash commands and invalid commands. Type text during a Run; emit slash-command text from a provider. | Commands follow the specified Task/Run transitions. No shell escape, queued execution, output-as-command or in-flight route/context mutation. |
| A03 | All | On each mandatory profile row, pin a supported model and perform a small approved analysis/text task; then pin an unavailable/unsupported or account-ineligible model. | Successful bounded output with actual profile/version/reported identity and measured-or-explicitly-unknown usage. Coding profiles use the default read-only envelope without granting writes; Ollama is local text only. Unsupported selections fail before task execution without substitution. |
| A04 | All | Supply a valid reported identity, then missing/mismatched identity or a reroute signal. | Requested/reported identities remain distinct. Missing/mismatched identity blocks qualification or stops an active Run; no conforming-success claim. Already submitted data/charges are not described as undone. |
| A05 | All | Use fixed candidate fixtures with excluded pairs, multiple models per provider and tied priorities. | Provider is eligible iff at least one model survives; provider chosen first, model second; repeatable tie-break and rationale; Task-bound provider/model do not change on later Runs. |
| A06 | All | Give an ambiguous task kind and an unqualified capability/context entry. | No hidden model-based classifier or remote request; request an explicit supported task kind and manual route selection. If no qualified class applies, no Run is eligible. Manual pins cannot bypass required eligibility data. |
| A07 | All | Choose lower-priced but task-ineligible model, unknown-price model, and different latency metadata. | Quality/capability/privacy/permission floors survive; unknown is not zero; only reviewed priorities apply. Sources/uncertainty are visible. |
| A08 | All | Conflict user ceiling, approved project policy, Task constraint and requested route. | Restrictive constraints win or execution is rejected with field-specific diagnostics. Repository/model/CLI text cannot expand policy. |
| A09 | All | Change endpoint/account/auth/billing, extension settings or project-policy fingerprint after preview. | Old eligibility and approval cannot authorize the changed configuration; revalidation/reapproval required before invocation. |
| A10 | All | No remote data policy; request remote model discovery/classification or remote execution. | No outbound request that is not covered by an approved policy. Authorized metadata-only probes transmit no unapproved Task/workspace content. |
| A11 | Local text | Use loopback Ollama with cloud enabled, cloud model/proxy, or unverified server setting; then a verified local configuration. | Local-only rejects unverified/cloud execution. A child-process variable alone cannot certify an already-running daemon. Verified profile records evidence and model digest. |
| A12 | Coding | Start with the default envelope; attempt file write, shell side effect, network access and out-of-scope read. | Default is read-only with no tool network or write/side-effect commands; native controls actually deny unapproved operations. No silent unsandboxed retry, permission broadening or leaked fake credential marker. |
| A13 | Write | Small bug fixture in a clean Git workspace; approve explicit edit/check scope and execute it. | Same coding adapter passes on both targets; inspectable before/after diff and actual check status; no commit/push/PR/deploy. User acceptance is separate. |
| A14 | Write | Start with staged, unstaged or untracked work; also exercise a read-only non-Git folder. | Write admission is rejected without stash/reset/cleanup. Read-only non-Git usage is possible when other constraints pass. |
| A15 | Coding/Write | Native operation requests scope beyond the approved envelope, including dependency install or new network access. | Deny and end as needs-approval; do not continue after broadening permissions in place. A new Run needs fresh preview/approval. |
| A16 | Coding | Repository rules/tool output instruct permission expansion; configure project/user hooks, MCP, plugins, background/child agents and unsandboxed escape. | Unapproved executable extensions stay disabled or profile fails. Untrusted content cannot answer approvals, change policy or launch out-of-envelope work. |
| A17 | All | Open Task after CLI restart; attempt native resume, old approval reuse, provider/model change or interrupted-turn replay. | Restore Task requirements only; next Run is fresh and newly approved. Same-Task route changes are rejected; new Task required. No replay/automatic handoff. |
| A18 | All | Provider proposes revised constraints/criteria; user later approves an explicit revision. | Proposal cannot overwrite approved fields; criterion IDs/revision provenance persist; only explicit user-approved revision affects later Runs. |
| A19 | All | Unknown schema, malformed required fields, wrong workspace, missing/changed required source; missing unused optional source. | Invalid/required stale data blocks before invocation. Optional unused source is labelled excluded/stale. No silent migration, repair or workspace reassignment. |
| A20 | All | Select bounded files explicitly, including Thai/Unicode content, spaces and unusual filenames. | Payload manifest/fingerprints match previewed content; no shell interpolation or automatic relevance crawl; Ollama receives text without file tools. Estimates declare their method/uncertainty. |
| A21 | All | Prepared payload exceeds its configured allowance/output reserve. | Reject before provider call; no truncation of constraints/criteria, invisible clipping or automatic compaction. |
| A22 | All | Native context overflow/compaction event or truncated/failed native result. | Preserve approved requirements and record the reported event/uncertainty; stop overflow without retry or harness compaction. Do not claim full visibility into native context internals. |
| A23 | All | Set max Runs to one; begin a Run that fails or is cancelled; restart CLI and try again. | Attempt remains consumed; second admission blocked; local preflight failures do not consume an execution attempt. |
| A24 | All | Existing spend/reservations 0.05, next estimate 0.06, monetary allowance 0.10; reconcile a prior reservation once. | Reject admission; reservations survive restart and are not double counted or automatically refunded. Monetary numbers remain labelled by evidence type. |
| A25 | All | Unknown price/quota or missing final usage; first deny, then explicitly authorize the exact unknown-cost profile. | Default no unknown-cost admission; approved exception still has attempt/time caps. No zero/free substitution or complete-looking total; uncertain reservations retained. |
| A26 | All | Provider reports actual/estimated tokens and client-side estimated cost, or omits them on failure. | Preserve measurement source, units, pricing revision and unknown fields. No client estimate presented as invoice truth or missing value as zero. |
| A27 | All | Cancel mid-stream, during a tool action and during an approval wait; repeat using timeout. | Graceful stop within 5 seconds then forced owned-tree/stream cleanup within another 5 seconds; no new work; incomplete/terminal reason and usage uncertainty recorded. |
| A28 | All | Cleanup or remote cancellation cannot be confirmed. | Mark cleanup/Run indeterminate; block conflicting work; do not claim rollback, remote termination or stopped billing. |
| A29 | All | Auth failure, rate limit, provider unavailable, malformed/partial JSON and missing terminal result. | Typed stop-and-report outcome; no harness retry, route/billing substitution or duplicate side effects. Native internal retries are characterized separately. |
| A30 | All | State directory is unwritable/full before admission and becomes unwritable during execution. | No invocation without durable admission; mid-Run failure triggers stop and an explicit incomplete-evidence warning. |
| A31 | All | Crash after admission; restart with unfinished Run, stale lock and possibly reused process ID. | No native resume/replay/refund; expose last durable state; require operator reconciliation; do not kill an unrelated process or silently assume completion. |
| A32 | All/Write | Two instances target same Task or simultaneous writes to one canonical workspace, including path aliases. | Conflicting execution rejected by shared locks. A lock is not treated as permission to bypass the envelope. |
| A33 | All | Seed fake credentials/PII in errors, command output and optional raw capture. Inspect harness storage. | Default journal/history contain no raw transcript/output or fake sensitive markers; permitted metadata minimized/redacted; raw capture only after explicit opt-in. |
| A34 | All | Inspect native filesystem footprint for a fresh Run with transcript persistence disabled. | List remaining logs/cache/temp artifacts and owners; no blanket zero-storage claim. Auth-mode compatibility tested independently; raw native retention requires opt-in. |
| A35 | All | Delete an inactive Task; try deleting an active Task and simulate partial deletion. | Preview/confirm and remove owned artifacts/indexes only; active deletion denied; workspace/provider homes/credentials/backups unaffected; partial failure is reported. |
| A36 | All | Provider exits zero and claims success while checks fail, are absent or have stale references; user reviews criteria at revision N, then approves changed criteria at N+1. | Separate Run completion, check evidence and Task acceptance; no self-reported verified success or rewriting of failed checks. Criterion decisions and acceptance bind to revision N and cannot automatically accept N+1. |
| A37 | Coding | Remove required sandbox dependencies, enable bypass/exclusions or use an unqualified provider/version/configuration. | Fail closed with recovery diagnostics; do not launch a less restricted fallback. Safety evidence is actual platform behavior. |
| A38 | All | Stream terminal escape/OSC sequences, approval-looking output, oversized events and malformed control bytes. | Safe bounded display; no clipboard/title/control injection, approval spoofing or output-triggered command. Clearly label display omissions. |
| A39 | Coding/Write | Attempt traversal/symlink escape, credential reads/inherited tool secrets, forbidden Git/remote operations and network through an approved test script. | Enforcement denies operations outside the envelope, including indirect script effects; a command name/allowlist label alone is not evidence of confinement. |
| A40 | All | Change required input/workspace/policy between approval preview and invocation; inspect unexpected concurrent edits. | Do not reuse stale approval. Record detectable concurrent changes and uncertainty; do not attribute external changes to the agent without evidence. |

## Release evidence and sign-off

Evidence package contains:
1. Complete support/conformance manifest with every mandatory row and enabled write capability.
2. Test results mapping A01–A40 to applicable rows, distinguishing live tests and injected failures.
3. Versioned task/model qualification fixtures, priorities and context/cost metadata sources.
4. One approved-edit demo on both OS targets with actual diff/check evidence and explicit human acceptance.
5. Known limitations, native persistence/data destinations, unresolved provider uncertainties and disabled features.
6. Setup/recovery/deletion instructions and a human MVP sign-off.

If a mandatory safety/compatibility row cannot pass, stop for a concrete scope/architecture decision. An accepted documentation proposal, a passing mock suite or a note listing limitations does not waive the release gate.
