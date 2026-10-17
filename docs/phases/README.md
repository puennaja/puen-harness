# Puen Harness — Phased Feature Specifications

Status: **Phase 1 documentation approved on 2026-10-17.** No implementation authorization or provider certification has been granted.

| Phase | Document | Focus | Gate |
| --- | --- | --- | --- |
| 1 | [Foundation & Model Selection](phase-01-foundation.md) | Terminal; Codex/Claude coding agents and Ollama text; manual/static Auto; minimal policy, safety, accounting and Portable Task Context v0 | [Conformance matrix and acceptance contract](phase-01-acceptance.md) |
| 2 | [Context & Workflow Handoff](phase-02-context-workflows.md) | Scoped retrieval, compaction, native resume, workflows/skills, cross-provider handoff and independent verification | Handoff integrity + context quality + evidence |
| 3 | [Evaluation & Learning](phase-03-evaluation-learning.md) | Routing improvement, verified memory, evaluation and expanded configuration UX | Reproducible quality/cost gains + human-approved policies |

## Product invariants across all phases

1. Codex and Claude retain their own coding-agent loops. Ollama is a bounded local text route in Phase 1; no harness-built agent loop. Broader local-agent integration requires review.
2. Auto chooses provider first, then model, after filtering provider/model pairs. Manual pins still require eligibility. Unknown cost is not zero; cost cannot override data, quality/capability or safety constraints.
3. Workflows reside in `puen-engineer`; skills in `puen-stack`. Harness consumes approved versioned references when Phase 2 integration is authorized, without duplicating their source of truth.
4. Durable user-approved Task Context is independent of native conversations. JSON is canonical; Markdown is generated. Stable requirements and evidence references survive CLI restarts.
5. Side effects require explicit human authorization and enforceable limits. Repository/model/tool text cannot grant permissions. Unknown enforcement fails closed.
6. Learning is evidence-based and human-approved. Model weight training and autonomous policy modification are excluded.
7. Provider completion, check results and human Task acceptance are separate states. Neither an agent self-report nor a zero exit status establishes verified success.

## Phase 1 review package

- [Behavior and scope](phase-01-foundation.md)
- [Acceptance scenarios and exact qualification-row contract](phase-01-acceptance.md)
- [Q1–Q34 decisions, risk register and source evidence](phase-01-review.md)
- [Domain glossary](../../CONTEXT.md)
- [Native agent boundary ADR](../adr/0001-preserve-provider-agent-loops.md)
- [Task Context authority ADR](../adr/0002-task-context-authority.md)

Phase 1 includes minimal policy precedence/validation (F17a), metadata journaling (F12a) and stop-and-report failure handling (F13a); their later-phase expansions cannot be prerequisites for initial safety.

## Approval and status

Approve the specification and acceptance criteria first. **Implementation and live provider executions need separate authorization.** After authorized development, the MVP exit gate evaluates actual evidence; documentation approval is not MVP acceptance.

- Phase 1: Q1–Q34 discussion complete; documentation approved on 2026-10-17; implementation not authorized; qualification not performed.
- Phase 2: Draft, not reviewed. Only inherited Phase 1 boundary clarifications are approved here.
- Phase 3: Draft, not reviewed. Only inherited Phase 1 boundary clarifications are approved here.

[Original consolidated backlog](../feature-spec.md) remains historical reference; its original routing modes and MVP demos are not the current Phase 1 acceptance contract. **Approved phase documents govern implementation scope.**
