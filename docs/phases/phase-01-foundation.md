# Phase 1 — Terminal Foundation & Model Selection

> Status: Documentation approved by the user on 2026-10-17 after the Q1–Q34 grill-with-docs discussion. Implementation and live provider execution require separate authorization. Provider conformance has not been performed.

## Goal and authority

Deliver one terminal for manual or deterministic Auto selection of Codex, Claude Code and Ollama, with explicit permissions, durable Portable Task Context v0 and honest usage reporting. Demonstrate one small approved coding task with an inspectable diff and actual check results.

This phase governs the approved MVP scope. [Shared vocabulary](../../CONTEXT.md), [acceptance contract](phase-01-acceptance.md) and [decision/risk record](phase-01-review.md) are companion documents. Phase 2/3 remain unreviewed except for the boundary clarifications identified in that record.

There are three separate gates:
1. **Documentation approval:** confirm this specification and its acceptance contract.
2. **Implementation authorization:** separately authorize development and any provider executions. Documentation approval alone grants neither.
3. **MVP acceptance:** after implementation, accept real conformance evidence for the declared support matrix.

## MVP boundary

| Area | Phase 1 contract |
| --- | --- |
| Platforms | Qualification targets: Ubuntu 24.04 LTS x86_64 and macOS 15 Apple Silicon. Support is claimed only for exact tested OS/provider/configuration rows; other OS versions and architectures remain unqualified. |
| Concurrency | One active Run per harness instance. Cross-instance locks prohibit overlapping Runs for one Task and concurrent writes to one canonical workspace. |
| Coding providers | Codex and Claude Code retain their native agent loops. Both must qualify for the baseline on both targets. At least one same coding adapter must qualify for approved edits on both targets. Write capability is granted per tested profile, never inherited from another authentication mode or platform. |
| Authentication | First-party native subscription login and first-party direct API are separate qualification rows for both coding adapters. No automatic account, endpoint, credential or billing-mode substitution. Gateways, Bedrock and Vertex are excluded. Login remains with the native provider; harness does not become a credential store. |
| Ollama | Local text tasks using already installed, qualified models; no agent-owned filesystem/shell tools or harness-built agent loop. The operator must supply trusted evidence that the running service has cloud features disabled and uses a qualified local model. A loopback URL alone is insufficient. |
| Routes | Manual and Auto v0 (static balanced priorities). Local-only is an independent hard data constraint in either mode. |
| Writes | Require an approved Run and a clean Git workspace, including no staged or untracked work. No automatic stash, reset, commit, branch creation or worktree management. Read-only tasks may use non-Git folders. |
| Deferred | Native session resume, same-Task provider/model changes, cross-provider handoff, semantic retrieval, harness compaction, workflow/skill integration, fallback orchestration, learning and weighted/learned routing. |
| Excluded operations | Commit, push, PR creation, deploy, unrestricted shell execution and automatic model installation/download are outside Phase 1 execution scope. |

Missing installations or credentials are normal diagnostic results, not startup crashes. An operator needs only one eligible route to use the terminal; the release qualification gate still covers all required adapters/profiles.

Node/TypeScript + Ink remains a candidate, not a settled product requirement. Structured provider interfaces, SDKs or subprocess protocols may be selected only if they satisfy this contract while preserving native agent loops; terminal-screen scraping and assumed undocumented fields cannot establish safety or conformance.

## Features and dependencies

| ID | Included work |
| --- | --- |
| F01 | Terminal streaming, inspection, Task lifecycle, explicit approval and bounded cancellation. |
| F02 | Three adapters; structured events, model identity, permission enforcement, process cleanup and version/configuration conformance. |
| F03 | Installation/auth diagnostics, discovery with provenance, explicit pinning and unsupported-model errors. |
| F04 | Local deterministic task classification, eligibility filtering, provider-first/model-second selection and an inspectable rationale. |
| F05 | Approved permission envelope, prompt-injection boundaries and fail-closed execution. |
| F06a | Durable Task/Run state, fresh native sessions, workspace identity, crash handling and locks. Native resume moves to F06b in Phase 2. |
| F07 | Per-Task admission accounting, Run/time caps and measured/estimated/unknown usage. |
| F08a | Versioned, file-backed Portable Task Context v0, local generator/reader and explicit file selection. |
| F12a | Minimal private Run journal, redaction and explicit deletion. Rich evidence/handoff remains F12 in Phase 2. |
| F13a | Stop-and-report on failures, timeouts, cancellation and uncertain completion. No automatic retry/fallback. |
| F17a | Minimal validated user/project policy, precedence, approval fingerprints and diagnostics. Full policy UX remains Phase 3. |

## Task, Run and approval lifecycle

- A Task is a durable objective with user-approved requirements and acceptance criteria. It can have multiple Runs.
- Each Run binds one Task Context revision, provider/model, authentication/billing profile, effective policy and approval envelope. A Run is one explicit provider execution; internal native tool/model calls are not additional harness Runs.
- Select a route before the first Run; it becomes the Task's fixed provider/model route when the first Run is admitted. Subsequent Runs must revalidate that route. A different provider/model requires a new Task; no silent rerouting of an existing Task.
- Every Run starts a fresh native session. Opening a Task after restart restores its approved requirements and journal, not a native conversation or old permissions. No automatic history transfer, summary, replay or provider handoff.
- Locally draft requirements and acceptance criteria, then show context, route, data destination, permissions, estimates/unknowns, persistence and limits in one approval preview. No provider invocation occurs merely by typing a request.
- Requirement changes create a new immutable revision with user approval; previous revisions remain inspectable until explicit deletion. Stable criterion/constraint IDs are not silently reassigned.
- A Run approval expires when the Run ends, is cancelled, times out or loses its binding state. It does not survive restart or authorize a later Run.
- The default envelope is read-only with no tool network access or write/side-effect commands. Approving an analysis Run does not grant edit rights; a reviewed edit/check scope must explicitly grant the required operations.
- Native approval requests already inside the approved envelope may be answered according to that envelope. A request to expand scope is denied; end the Run as needs-approval and require a new preview, approval and Run. No live expansion of Run policy.
- Task acceptance is separate from Run completion and check results. Bind each criterion decision and Task acceptance to the exact Task Context revision reviewed. Earlier acceptance remains historical and cannot automatically accept a newer revision. A zero exit code or provider self-report cannot mark a Task accepted; failed/omitted checks stay visible. Acceptance does not rewrite evidence or waive safety constraints.

## Terminal behavior

| Command/input | Observable behavior |
| --- | --- |
| Normal text while idle | Draft the next Run for the current Task; show the approval preview before execution. The first request without a Task offers Task creation. |
| /task new | Create a draft Task in an explicitly selected workspace. |
| /task list, /task open <id> | List/open private Task state. Opening never starts or resumes provider work. |
| /task delete <id> | Preview the harness-owned data to remove and obtain explicit confirmation; refuse deletion of an active Task. Does not revert workspace edits or remove provider-owned data. |
| /provider [id], /model [id] | Inspect choices or select the initial route of a draft Task. On a route-bound Task, explain that a new Task is required; never mutate an active/bound route. |
| /auto | Select Auto for a draft Task; show classification, candidates, exclusions and priority rules before approval. It is not permission to execute. |
| /status | Show Task/Run state, requested/reported model, effective profile, limits, uncertainty and last known activity without mutating state. |
| /budget | Inspect usage/reservations and limits. Proposed changes require policy approval and apply only to subsequent Runs. |
| /context | Inspect/validate the canonical revision, sources, sensitivity and payload estimate. Draft changes locally; approved changes create a revision used only by a subsequent Run. |
| /cancel, Ctrl-C during a Run | Request cancellation; remain in the terminal after cleanup. Repeated cancel requests do not start work. |
| /exit, EOF, terminal disconnect | Stop any active Run using the same bounded cleanup procedure, then exit. Ctrl-C while idle exits. |
| Unknown command / input during a Run | Return a local validation error; no shell escape, implicit command execution, queued Run or in-flight context mutation. Status, context/budget inspection, cancellation and exit remain available. |

Provider output is data: it cannot execute slash commands, answer approvals, change terminal clipboard/title or inject terminal control sequences. Streaming buffers and individual events must be bounded; omitted display content is labelled, never confused with complete evidence. Credentials/PII must be redacted in persisted errors and journal fields as well as optional raw capture. Default history is in-memory only.

## Policy, privacy and execution safety

### Policy authority

The user-controlled policy outside the target repository is the permission ceiling. Project policy is a user-approved snapshot/fingerprint stored with private harness state. Repository files, model output, command-line route selection and provider preferences cannot expand it.

Effective policy is the restrictive intersection of the user ceiling, approved project snapshot, Task constraints, Run envelope and qualified adapter capabilities. A conflict blocks execution with field-specific diagnostics. Changes affecting permission, destination, billing, persistence, classification or limits require renewed project approval. Tighter constraints also require revalidation; an active Run's snapshot never changes.

No remote metadata probe, classification, prompt or model call is made until an approved policy covers that request and destination. Local diagnostics may inspect installed versions and local auth-state metadata without exposing credentials; they must not load untrusted executable project configuration. Authentication/endpoint changes invalidate route eligibility and approval.

### Data and tool boundaries

- Data authorization covers the entire workspace accessible to the agent, including native repository reads; selecting only a few initial files is not a confidentiality boundary.
- If any accessible project data is unauthorized for a provider, that remote coding route is ineligible. Phase 1 has no per-file DLP engine or guarantee of detecting all secrets.
- Local-only requires qualified on-machine inference, not merely an endpoint named local or a local proxy. Do not silently reconfigure/restart an existing Ollama service or download a model.
- Authorizing provider API traffic does not authorize network access by agent tools. Tools have no network permission by default. Additional destinations, dependency installation and out-of-workspace access require a new approved envelope/Run and must remain within Phase 1 scope.
- Credential access required by the trusted native provider is distinct from what agent tools/model inputs may access. A qualified profile must declare its runtime/authentication locations and prevent unintended credential exposure to tools, prompts, logs and inherited tool environments.
- The trusted boundary includes the operator, OS, harness and qualified provider runtime. Repository instructions, selected file content, tool output and model responses are untrusted and cannot change permissions.
- Qualified provider-native controls are permitted; extra OS containment is not universally required. Proof is per OS/version/authentication/configuration. A prompt, UI label or provider promise alone is not enforcement.
- Disable unapproved hooks, plugins, MCP servers, executable startup/shutdown extensions, automatic memories and child-agent/background execution in a dedicated harness configuration. No uncontrolled inherited settings or silent unsandboxed retry.
- A profile unable to enforce an envelope is ineligible, even if that leaves no coding route. Do not fall back to an unsafe mode. Unsupported operations remain blocked despite a route pin or lower price.

Workspace binding uses canonical identity and source fingerprints. Path traversal/symlink escapes must be covered by actual enforcement tests, not only startup path checks. Operators must avoid concurrent external workspace edits. Changed required inputs between preview and start invalidate approval; unexpected concurrent changes during execution must be surfaced and must not be attributed to the agent without evidence.

## Discovery, model identity and routing

A conformance profile identifies adapter/provider version, structured-interface version, OS/architecture, authentication and billing mode, endpoint/account identity reference, effective permission/extension settings, persistence behavior, supported capabilities and evidence. Store secret-free identifiers only.

Discovery distinguishes installed, authenticated, discoverable, qualified and eligible. Availability is account/configuration dependent. Curated metadata is versioned and labelled; it is not presented as a live discovery result. Unknown required capabilities or context capacity cannot be invented. Missing optional usage remains unknown.

Resolve aliases to a concrete qualified model identity before task execution where supported. Capture requested and provider-reported model identity, including Ollama digest where available. A tested identity signal is mandatory for manual pin/Auto conformance. If identity is missing/mismatched or a reroute is reported, stop and record model-mismatch or indeterminate as applicable; do not accept the output as a conforming Run. Detection after submission cannot undo prior transmission, processing or charges.

Auto v0:
1. Classify locally using explicit task kind or deterministic rules. For ambiguous/unclassified work, ask the operator to declare a supported task kind and choose a route manually. Manual selection does not waive task-class qualification; if no qualified class applies, no Run is eligible.
2. Filter provider/model pairs by installation/auth, qualification, data policy, permissions, tooling, task-class qualification, context capacity and budget.
3. A provider is eligible only when at least one compatible model survives.
4. Choose the provider using a reviewed, versioned task-specific priority list; choose a surviving model using that provider's task-specific model priority list.
5. Make priorities and tie-breaking explicit. Use numeric priorities, with canonical provider/model IDs as deterministic tie-breaks.
6. Preview selected route and exclusions, known/unknown cost/context/latency and registry/rule versions. No learned score or hidden model-based classifier.

The eligibility registry must contain task-class qualification fixtures/evidence, effective input/output allowance and freshness, model identity, capabilities and priorities before Auto can use a row. Small fixtures support bounded eligibility claims, not a general model-quality guarantee. Prices and latency do not invent quality, allow unknowns to count as zero, or override constraints. Actual elapsed latency is measured; predictions without evidence remain unknown.

## Portable Task Context v0

JSON is canonical. Markdown is a generated read-only view; it never becomes a second editable authority. Provider output cannot overwrite approved requirements.

Required fields for the Task Context v0 schema:

| Group | Required content |
| --- | --- |
| Identity | schema_version, task_id, revision, created_at, approved_at/approval status, canonical workspace identity and author/provenance. |
| Requirements | goal, scope, stable-ID acceptance_criteria and hard_constraints; at least one criterion. |
| State | decisions and unresolved_questions, each with origin and status; empty arrays are valid. Safety/routing/required-acceptance uncertainties block affected execution; nonblocking questions are visible. |
| Sources | source_refs with stable identity, workspace-relative location where applicable, content fingerprint or version/commit, freshness, origin/trust label, sensitivity and required/optional status. Empty sources are valid for self-contained text tasks. |
| Data policy | Project policy reference/fingerprint, sensitivity/locality constraints and approved data destination scope. |
| Context | Configured payload budget, prepared-payload estimate/method/uncertainty and effective qualified context capacity/output allowance. |
| Evidence | References to Run/check/diff/decision records with provenance and verification status; empty before execution. |

At preparation time the harness may include text from explicitly selected, approved workspace files, with a bounded payload and previewed source manifest. It performs no relevance search or implicit file crawling. Ollama receives only this prepared text, not file/shell tools. Native coding agents may read the authorized workspace under the approved envelope.

Before every Run validate schema, required fields, project/workspace identity, policy/revision binding, source freshness and payload capacity. Unsupported schema, missing required sources, changed required content or over-budget payload blocks execution and explains the corrective action. Revalidation of changed material creates an approved revision; never silently truncate constraints/criteria or migrate versions.

Budget here covers the payload the harness prepares, with a disclosed output/working reserve. It is not a claim that the harness controls all native tool output or provider history. Native agent context management remains native; qualify applicable limit behavior and report native overflow/compaction signals where available. A native overflow stops the Run without automatic retry or harness compaction.

## Budgets and usage

- Default maximum is **3 admitted Runs per Task**, **15 minutes per Run**, including any in-Run approval waits. Limits are configurable only through validated, approved policy; no unbounded default.
- Monetary admission limits are operator-configured. Require either a finite estimate and configured monetary allowance or explicit project permission for the identified unknown-cost billing profile. Attempt/time limits apply in both cases.
- Count a Run and durably reserve its estimated allowance before invoking a provider. A local validation rejection before invocation is not an admitted Run.
- Sum usage/reservations across Task Runs and CLI restarts. No automatic refund of attempts on failure/cancellation/crash. Do not double count a reservation and its reconciled usage.
- Reject admission when attempt limits are exhausted or known/reconciled amounts plus outstanding estimated reservations and the next estimate exceed the configured monetary allowance.
- Missing final usage retains uncertainty and the outstanding estimate/reservation; never replace it with zero or silently free it after restart. Corrections require an explicit, recorded operator reconciliation.
- Unknown-cost routes require the approved provider/endpoint/auth/billing policy from Q13. Report any monetary subtotal as incomplete while unknown components exist; no claim that a monetary ceiling is enforceable for those components.
- Separate measured token counts, estimated tokens, client-side cost estimates, externally confirmed amounts, subscription/quota uncertainty, and missing values. Record source, method and pricing revision. Local inference is not described as zero total economic cost.
- Estimates are admission control, not guaranteed final spending limits. In-flight native calls/tools can overrun and provider-side internal activity can affect cost. Record overruns and block later admissions when limits are reached.
- No automatic harness retry/fallback, recursive agent calls or new model calls for classification/context drafting. Internal provider retries/tool calls are not falsely counted as separate harness Runs: characterize and bound them where supported in qualification, disclose remaining uncertainty, and retain Run timeout limits.
- Cross-Task daily/global monetary caps are deferred; Phase 1 per-Task accounting does not claim to prevent spending across newly created Tasks.

## Run outcomes, cancellation and recovery

Persist a terminal reason separately from validation and user acceptance. Reasons include completed, failed, cancelled, timed-out, needs-approval, model-mismatch and indeterminate. A completion event/process exit does not certify task correctness. Missing/malformed completion, partial JSON or lost transport must not be converted into success. Unknown optional informational fields may be ignored without inventing permissions, identity, usage or terminal status.

Use up to **5 seconds** for graceful interrupt/cancellation, then up to **5 seconds** for forced cleanup of the harness-owned process tree/stream. No new tool action or Run may be launched as a cancellation side effect. Lack of confirmed termination leaves an indeterminate state, visible process/lock warning and blocked conflicting work pending operator review.

Cancellation/timeout does not roll back files, prove that a remote request stopped, or guarantee billing stopped. Preserve partial-change metadata, usage uncertainty and the reason. A timed-out Run with unconfirmed cleanup also records cleanup as indeterminate.

Auth failure, rate limit, unavailable provider, context overflow, denied permissions, disk failure or partial writes stop and report. Offer local inspection and an explicit new-Run action only after revalidation. Never replay writes, switch route/billing, retry automatically, continue an interrupted turn or auto-clean the workspace.

On startup find unfinished admissions and locks; report their last durable state and require operator review. Do not reconnect/resume, reissue requests, auto-release uncertain reservations or kill unrelated/reused PIDs. Fail closed before invocation if durable state/locks cannot be written; stop and surface incomplete evidence if persistence fails mid-Run.

## Storage, retention and deletion

Use file-backed Task/revision/Run artifacts in a user-private local directory outside target repositories; the eventual OS-specific path is an implementation choice documented in setup. Restrict access to the owner. Protect durable updates against partial writes and concurrent instances.

Default harness storage contains approved Task Context, source/evidence references, model/profile/policy versions, route rationale, approvals, terminal states, changed-path/diff references, check command/exit status, usage availability and sanitized errors. It does not contain full raw conversation, full selected source copies or raw tool/provider output by default. Task Context itself is user-approved sensitive content and must be minimized and access-controlled.

Optional raw capture requires explicit persistence approval, redaction and declared destinations. If disabled, large check outputs can be inspected in the current terminal or linked to operator-approved external evidence; absence of a retained log must be explicit. Do not label a missing or stale reference as verified evidence.

Prefer qualified no-native-transcript modes. Declare remaining native logs/caches/temp artifacts and their ownership before profile approval; raw native persistence requires explicit opt-in. No assumption that one flag eliminates all native artifacts or is compatible with every auth mode. Provider-side retention/billing policies are separate and must be linked for the selected account/billing mode.

Deletion previews and removes only harness-owned Task artifacts and indexes, never an active Task, workspace changes, shared native provider homes, credentials, external logs/backups or provider-side/billing records. Report incomplete deletion honestly. Automatic expiry/retention jobs and broad native-data cleanup are deferred.

## Deliverables and exit gate

Deliver terminal/setup docs; three adapters and conformance manifest; versioned minimal policy/routing registry; Task Context schema/local generator/reader/renderer; private journal/accounting/locking/deletion; and [the acceptance evidence package](phase-01-acceptance.md).

Exact provider/interface/configuration versions and model rows are filled from measured qualification, not guessed from current documentation. That is required engineering evidence, not authorization to silently relax scope. First qualify the mandatory permission, auth, model-identity, locality and persistence contracts. If no compliant integration exists, stop and return a concrete scope/architecture change for human decision.

MVP acceptance requires the declared support matrix, manual/Auto behavior, small approved coding demo on both targets using at least one same coding adapter, Task Context continuity and negative safety/failure tests. Mocks alone cannot certify native permissions or compatibility. The human signs off the actual evidence and outstanding limitations; unqualified capabilities stay disabled.
