# Phase 2 — Context Engine, Workflow & Cross-provider Handoff

> Status: Draft, not reviewed. Only the inherited Phase 1 boundary notes below were approved with Q1–Q34 on 2026-10-17; the remaining Phase 2 design still needs its own grill-with-docs review. Specification only, not implementation authorization.

## Goal
Transfer well-scoped work between distinct AI providers without copying entire chat histories; preserve requirements, source provenance and verification evidence as context is condensed.

## Dependencies
Phase 1's adapter contract, Task Context v0, minimal policy/approval enforcement, accounting and journal must be accepted. Phase 2 cannot bypass human gates or reimplement underlying coding-agent loops.

## Inherited Phase 1 boundary
- Minimal local file selection, payload validation, source freshness/schema checks and Task Context persistence already belong to Phase 1 F08a. Phase 2 adds relevance-based retrieval, compaction and migration.
- Phase 1 uses a fresh native session per Run and binds a Task to one provider/model. Native resume and same-Task provider/model continuation need the explicit compatibility, permission-reset and accounting contracts reviewed in this phase.
- Stop-and-report on auth/rate-limit/context/timeout/partial-write/uncertain-completion failures is mandatory in Phase 1 F13a. This phase adds guided recovery/fallback, without automatic replay or silent route/billing changes.
- Minimal private journal, deletion scope and artifact ownership are defined in Phase 1 F12a. This phase adds richer evidence/export rather than postponing basic auditability.
- Existing workflow/skill policies remain authoritative in their source repositories; controlled consumption begins here.
- References to verified lessons below mean explicitly reviewed external records until Phase 3 learning storage is available; they do not authorize a Phase 2 autonomous memory store.

## Scope / feature IDs
- F06b Native resume/continuation: capability-bounded restoration only after validating context, workspace, route/auth/version, current permissions, fresh approval and cumulative Task accounting. No assumption that old approvals or interrupted writes can be replayed.
- F08b Context Engine: scoped retrieval of approved project rules (`AGENTS.md`), relevant files, tasks, architecture decisions, reviewed lessons and references. Rank relevance/freshness, remove duplicates, label untrusted sources, redact secrets, honor per-step token budget; provider may still inspect the authorized repository itself.
- F09 Context compaction/checkpoints: preserve goal, acceptance criteria, hard constraints, decisions, unresolved issues, approvals and evidence refs. Allow inspect/rebuild/restore, record dropped/retained material; detect pack overflow and stale refs.
- F10 Workflow/skill integration: consume **version-pinned** workflows from `puen-engineer` and skills from `puen-stack`, with logged stage boundaries, human gates and no uncontrolled skill installation.
- F11 Structured cross-provider handoff: task ID, objective, constraints, changed files and commit identity, plan, decisions, verification status, unresolved issues, next actions, provenance, source freshness and provider capability differences. Handoff is artifact-based, never provider-internal session serialization.
- F12 Evidence journal: extend the Phase 1 metadata journal with per-step files touched, decisions, approved test logs, verification result, token/cost estimates and human approvals with redaction/export suitable for pilot docs. Respect persistence policy.
- F13 Safe fallback/recovery: extend Phase 1 stop-and-report handling for rate limit/auth/context/timeout/partial-write scenarios. Any retries must be explicit, bounded and avoid duplicate side effects. Material route, billing or permission changes require consent.

## Deliverables
Context Pack v1 schema/version migration; retrieval/scoping pipeline; compaction/checkpoint artifacts; native-resume compatibility contract; handoff envelope/validator; workflow integration hooks; traceable evidence package; failure recovery runbooks and tests.

## Definition of done — observable scenarios
1. A task spanning BE/BFF receives only relevant source refs/rules under a configured budget; sensitive/untrusted files do not silently override user instructions.
2. After compaction, every original hard constraint and acceptance criterion can be recovered via structured checks; source refs remain resolvable or are marked stale.
3. Planner using Claude produces a portable plan; Codex continues via handoff artifact and implements without requiring full Claude transcript. Reverse handoff is supported in capability-bounded fashion.
4. Independent verifier reads **actual diff/tests/requirements**, identifies a deliberately inserted bug despite implementer's optimistic summary, and records pass/fail evidence.
5. Handoff with stale branch SHA, missing required decision or unknown version is rejected or explicitly flagged for revalidation.
6. A failed provider call or context overflow does not loop indefinitely or silently repeat writes; human receives retry/fallback decision.
7. Workflow from `puen-engineer` and skill from `puen-stack` have stable version refs; consuming repo files cannot bypass permission gates.
8. Pilot run creates redacted, inspectable artifacts containing traceable rationale for route, edits, checks and handoff.
9. A supported native resume revalidates permissions and sources, obtains fresh approval, preserves cumulative accounting and does not replay an interrupted side effect.

## Non-goals
Unbounded semantic memory, vector database by default, always-on multi-agent execution, automatic skill/rule rewriting, production release automation.

## Decisions to grill
- Minimal vs optional fields of handoff envelope, and compatibility/versioning policy?
- How do we evaluate retrieval and compaction *quality*, not just token savings?
- When should an agent rely on path pointers rather than injecting file content?
- How should richer/optional evidence be stored and deleted within Phase 1's artifact-ownership boundary?
- Which workflow gates are fixed human approvals vs configurable local policies?
- Which native resumes and same-Task route changes can meet the inherited safety/accounting contract?

## Exit gate
After separate specification approval and implementation authorization, demonstrate cross-provider task completion with preserved constraints and independent verification; human approves context-quality and failure/recovery evidence. Phase 1 acceptance does not approve this phase.
