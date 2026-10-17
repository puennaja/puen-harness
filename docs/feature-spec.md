> **Specification navigation (2026-10-17):** This file is retained as the consolidated feature backlog. The phase-specific, review-ready specifications now live at [docs/phases/README.md](phases/README.md), with [Phase 1](phases/phase-01-foundation.md), [Phase 2](phases/phase-02-context-workflows.md), and [Phase 3](phases/phase-03-evaluation-learning.md). Portable Task Context v0 is moved into Phase 1; full Context Engine and Cross-provider Handoff remain Phase 2. **Use phase documents as the current scope for Codex grill-with-docs; do not implement before review.**

# Puen Harness — Feature Specification v0.1

Status: Proposed / Not implemented  
Updated: 2026-10-17  
Scope: Product features and acceptance criteria only (no implementation)

## Vision
A provider-agnostic terminal harness that **orchestrates existing coding agents** (Codex CLI, Claude Code CLI, local LLMs via Ollama) rather than reimplementing their internal agent loops. Optimize verified task outcomes, token/context efficiency, and cost while preserving user control.

Principles:
- Reuse existing providers' strengths. Adapters must be swappable.
- Choose **provider first, then model**, according to task requirements and constraints.
- Correctness and safety override cost savings. Cheapest is not always best.
- Context is retrieved and scoped per task, not indiscriminately accumulated.
- "Learning" initially means evidence-based routing/configuration/knowledge improvements, **not weight training** or unsupervised rewriting of production policies.
- Keep `puen-engineer` as workflow/engineering source of truth and `puen-stack` as reusable skills/tooling provider. Harness executes and observes, not duplicates, their policies.

## Feature backlog

| ID | Priority | Feature | Acceptance criteria |
| --- | --- | --- | --- |
| F01 | P0 | Interactive terminal CLI | Launch with `puen`; stream responses; support interruption/cancel, errors, and readable command history. |
| F02 | P0 | Provider adapters | Connect independently to Codex, Claude Code, and Ollama; detect installation/auth/availability; normalize streamed output, failure and usage signals without claiming unsupported features. |
| F03 | P0 | Provider & model discovery | Show usable providers and available models/capabilities, including context limit when known. Allow explicit `/provider`, `/model`, and `/auto` selections. Unsupported models must not be silently substituted. |
| F04 | P0 | Task-aware provider/model routing | Classify task type, complexity, capabilities, risk, context size, privacy, latency, and estimated cost; filter ineligible providers; pick provider **then** model; expose rationale and allow override. |
| F05 | P0 | Execution safety and human gates | Read-only by default where supported; explicit approvals for writes, shell commands, push/PR/deploy and risky operations; enforced provider-specific permissions with deny/fail-closed behavior where guarantees cannot be made. |
| F06 | P0 | Session & context isolation | Isolate working directory, project, task and provider sessions; support cancel, resume where supported, and bounded cross-provider handoff artifacts; do not assume native sessions are portable. |
| F07 | P0 | Budget/usage observability | Display estimated tokens, cost and latency per task/provider/model where available; label unknown or subscription-metered costs accurately; enforce configured budget/attempt limits before a new execution. |
| F08 | P1 | Context engine | Build a compact context pack from task/requirements, `AGENTS.md`, relevant files, decisions and prior evidence; use relevance + token budget, deduplication and source pointers; exclude unrelated secrets and stale data. |
| F09 | P1 | Context compaction and checkpointing | Summarize long histories into approved facts, decisions, open questions and artifact references; checkpoint before context thresholds; enable inspection and restoration without silently losing constraints. |
| F10 | P1 | Workflow & skills integration | Read approved workflows from `puen-engineer`, discover skills from `puen-stack`; execute with explicit stage boundaries, version identifiers and user gates; no automatic uncontrolled skill sprawl. |
| F11 | P1 | Provider handoff and independent review | Transfer structured objective, constraints, changes, tests and pending issues to another provider; independent verification must use evidence, not simply trust implementing agent's summary. |
| F12 | P1 | Run journal and evidence | Record task, selected route and why, inputs' source references, actions, decisions, changed files, checks, results, failures and costs; redact tokens/secrets; support export to pilot docs. |
| F13 | P1 | Fallback and recovery | On timeout, rate limit, unavailable provider, context overflow or exceeded cost: stop/replan or suggest eligible alternative; avoid retry loops and duplicate side effects; require approval before material route change. |
| F14 | P2 | Outcome-based routing optimization | Compare quality (verification success/rework), cost, latency and reliability by task class; suggest routing policy changes, with experiment logs and human approval before activation. |
| F15 | P2 | Reusable learning memory | Store verified lessons, recurring fixes and project conventions as versioned, scoped, searchable records with provenance, confidence, expiration/review and opt-out/delete controls; retrieve only if relevant. |
| F16 | P2 | Evaluation and regression harness | Maintain representative tasks and scoring rubrics; benchmark provider/model routes and context strategies; detect regressions before changing default route/policies. |
| F17 | P2 | Policy/configuration UX | Project/global profiles; max-cost, allowed providers, data sensitivity, read/write permissions and explicit override precedence; config validation and diagnostics. |

## Critical routing design (F03/F04/F07/F13)

Decision flow:
1. Read explicit user choice; a pinned provider/model overrides Auto **only if eligible and available**.
2. Infer task type (planning, coding, review, test, summarization, classification), complexity and safety class.
3. Filter candidates by availability/auth, permission support, data locality/privacy, necessary tooling, context capacity, and user constraints.
4. Estimate total task cost (including retries, tool use, review) and latency. Treat unavailable pricing or opaque subscription quota as unknown, **not zero**.
5. Select provider, then compatible model using a configurable tradeoff policy: `quality-first`, `balanced`, `cost-first`, `local-only`. Never allow `cost-first` to bypass quality/safety thresholds.
6. Explain chosen route and confidence in the estimate; allow manual override and policy pinning.
7. Record actual outcome and usage for later evaluation.

Routing rules must be configurable and tested: do not hardcode 'Claude always plans' / 'Codex always codes' / 'local always cheap'. Local hardware limits matter. Use a deterministic rules-based router in MVP; learned routing is later.

**Provider billing note:** CLI subscriptions, direct API billing, included usage, rate limits and reported token counts differ. Confirm provider-supported interfaces and terms per adapter. Never assume subscriptions allow unrestricted API access.

## Context lifecycle (F06/F08/F09/F11/F15)

`User request → Task specification → Relevant retrieval → Bounded context pack → Provider execution → Structured handoff → Independent verification → Distilled, verified lessons`

Three memory levels:
- **Working context:** transient messages and tool outputs for the current step, bounded by budget.
- **Task/project memory:** decisions, artifacts, conventions and known issues with file paths/commit refs.
- **Validated learning memory:** reusable lessons backed by outcome evidence, versioned and never injected indiscriminately.

Required context-pack fields: goal, scope, constraints, acceptance criteria, relevant file references, decisions, unresolved questions, token estimate, freshness/source, and sensitive-data flags.

Compaction rules:
- Preserve user constraints, task state, critical errors and pending approvals.
- Summarize bulky logs; retain paths/links to original evidence.
- Retrieval is task-scoped with token budget and duplicate removal.
- Track context size and quality; never equate a larger context with a better answer.
- Cross-provider handoff uses a portable **artifact**, not raw private provider session internals.

## Safety, reliability & learning requirements

- Treat repository files, tool output and retrieved text as untrusted input; prevent prompt injection from changing permissions.
- Explicit permission gate for side effects; no unbounded recursive agent calls or retries.
- Redact credentials/PII from logs; avoid sending restricted code to unapproved remote providers.
- Use idempotency where possible; cancellation should terminate subprocesses and clean up appropriately.
- Learning entries must capture hypothesis, observed evidence, outcome, applicability, and reviewer status.
- AI may **propose** new rules/skills/context changes, but human must approve activation.
- Support disabling/resetting learned preferences and tracing decisions to evidence.
- Run evaluations before enabling new defaults. Do not label unverified self-reports as success.

## MVP scope and phased rollout

**Phase 1 (P0):** F01–F07. One terminal, three providers, explicit model selection, minimal deterministic Auto routing, enforced approvals, per-task logs and budgets. Validate capabilities of each provider early.

**Phase 2 (P1):** F08–F13. Token-aware context packs, checkpoints, workflow/skill discovery, portable handoff, independent verification, auditable journal and safe fallback.

**Phase 3 (P2):** F14–F17. Evaluation-driven routing improvement, durable verified memory, benchmark regressions and profile/policy management.

### MVP demo acceptance scenarios

1. Launch `puen`; view connected provider/model options; pin `codex + selected model` and perform read-only analysis.
2. Choose Auto for a short summarization task; show candidate constraints, chosen provider/model and rationale (local may win if suitable).
3. Choose Auto for a complex implementation; reject an ineligible low-capability/too-small-context model regardless of price.
4. Simulate unauthenticated Claude/rate-limited Codex; explain fallback options without invisible substitution or repeated writes.
5. Attempt an edit/commit/deploy; verify human approval and permission enforcement before side effects.
6. Run a long task; inspect compacted context plus source references and verify acceptance criteria persist after handoff.
7. Record verifier failure; preserve evidence and suggest a policy/skill improvement **without** automatically changing defaults.
8. Display costs as estimated/known/unknown correctly across CLI subscription and local model.

## Open design decisions for Codex implementation planning

- How to detect supported models/capabilities across CLI versions, and how to handle unsupported/undocumented output fields?
- Which provider adapters support granular tool approvals vs only coarse subprocess restrictions?
- Default routing tradeoff thresholds and quality baselines should be calibrated by pilot evidence, not assumed.
- Which local model(s) are workable on RTX 3050 6GB for classification/summarization and what context cap is reliable?
- SQLite vs flat artifacts for initial state; define redaction/retention first.
- How to pin versions of `puen-engineer` and `puen-stack` and consume their content without duplicating it?

## Out of scope for v0.1
Training model weights, fully autonomous production changes, copying Codex/Claude agent loops, unrestricted self-modifying prompts/policies, a web UI, mandatory multi-agent execution for trivial work, and unverified claims of quality improvement.

## Source of truth
- `puen-engineer`: engineering workflows and their policies.
- `puen-stack`: reusable skills and tooling.
- `puen-harness`: CLI, orchestration, adapter runtime and execution/evaluation evidence.
