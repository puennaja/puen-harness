# Phase 2 — Context Engine, Workflow & Cross-provider Handoff

> Status: Draft for `grill-with-docs`. Specification only; not authorization to implement. Prioritize observable acceptance tests and raise open questions rather than assuming answers.

## Goal
Transfer well-scoped work between distinct AI providers without copying entire chat histories; preserve requirements, source provenance and verification evidence as context is condensed.

## Dependencies
Phase 1's adapter contract, Task Context v0, permission policy and journal stable. No phase-2 feature may bypass human gates or reimplement underlying coding agent loops.

## Scope / feature IDs
- F08b Context Engine: scoped retrieval of approved project rules (`AGENTS.md`), relevant files, tasks, architecture decisions, verified lessons and references. Rank relevance/freshness, remove duplicates, label untrusted sources, redact secrets, honor per-step token budget; provider may still inspect repository itself.
- F09 Context compaction/checkpoints: preserve goal, acceptance criteria, hard constraints, decisions, unresolved issues, approvals and evidence refs. Allow inspect/rebuild/restore, record dropped/retained material; detect pack overflow and stale refs.
- F10 Workflow/skill integration: consume **version-pinned** workflows from `puen-engineer` and skills from `puen-stack`, with clearly logged stage boundaries, human gates and no uncontrolled skill installation.
- F11 Structured cross-provider handoff: task ID, objective, constraints, changed files and commit identity, plan, decisions, verification status, unresolved issues, next actions, provenance, source freshness and provider capability differences. Handoff is artifact-based, never provider-internal session serialization.
- F12 Evidence journal: per-step selected route, files touched, decisions, test logs, verification result, token/cost estimates and human approvals with redaction/export suitable for pilot docs.
- F13 Safe fallback/recovery: rate limit/auth/context/timeout/partial-write scenarios; bound retries and avoid duplicate side effects. Material changes to route or permission require consent.

## Deliverables
Context Pack v1 schema/version migration; context retrieval/scoping pipeline; compaction/checkpoint artifacts; handoff envelope/validator; workflow integration hooks; traceable evidence package; failure recovery runbooks and tests.

## Definition of done — observable scenarios
1. A task spanning BE/BFF receives only relevant source refs/rules under a configured budget; sensitive/untrusted files do not silently override user instructions.
2. After compaction, every original hard constraint and acceptance criterion can be recovered via structured checks; source refs remain resolvable or are marked stale.
3. Planner using Claude produces a portable plan; Codex resumes via handoff artifact and implements without requiring full Claude transcript. Reverse handoff is supported in capability-bounded fashion.
4. Independent verifier reads **actual diff/tests/requirements**, identifies a deliberately inserted bug despite implementer's optimistic summary, and records pass/fail evidence.
5. Handoff with stale branch SHA, missing required decision or unknown version is rejected or explicitly flagged for revalidation.
6. A failed provider call or context overflow does not loop indefinitely or silently repeat writes; human receives retry/fallback decision.
7. Workflow from `puen-engineer` and skill from `puen-stack` have stable version refs; consuming repo files cannot bypass permission gates.
8. Pilot run creates redacted, inspectable artifacts containing traceable rationale for route, edits, checks and handoff.

## Non-goals
Unbounded semantic memory, vector database by default, always-on multi-agent execution, automatic skill/rule rewriting, production release automation.

## Decisions to grill
- Minimal vs optional fields of handoff envelope, and compatibility/versioning policy?
- How do we evaluate retrieval and compaction *quality*, not just token savings?
- When should an agent rely on path pointers rather than injecting file content?
- Where do task artifacts reside relative to target repo and harness repo?
- Which workflow gates are fixed human approvals vs configurable local policies?

## Exit gate
Demonstrate cross-provider task completion with preserved constraints and independent verification; human approves context-quality evidence and failure/recovery behavior.
