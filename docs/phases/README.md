# Puen Harness — Phased Feature Specifications

Status: **Draft / For grill-with-docs review**, not implementation authorization.

| Phase | Document | Focus | Gate |
| --- | --- | --- | --- |
| 1 | [Foundation & Model Selection](phase-01-foundation.md) | Terminal, Codex/Claude/Ollama, provider-first/model-second routing, safety, budgets, **Portable Task Context v0** | Provider conformance + manual/Auto route + safety |
| 2 | [Context & Workflow Handoff](phase-02-context-workflows.md) | Scoped retrieval, compaction, workflows/skills, cross-provider handoff, verification | Handoff integrity + context quality + evidence |
| 3 | [Evaluation & Learning](phase-03-evaluation-learning.md) | Routing improvement, verified memory, evaluation, config | Reproducible quality/cost gains + human-approved policies |

## Product invariants across all phases
1. Existing Codex, Claude and Ollama engines remain independent; harness orchestrates, never replaces their agent loops.
2. Provider chosen before model in Auto mode; manual pins only if eligible; unknown cost is not free; quality/safety/privacy cannot be trumped by low cost.
3. Workflows reside in `puen-engineer`; skills in `puen-stack`; harness consumes versioned references.
4. A portable, durable Task Context has stable constraints/acceptance criteria and evidence references; native provider conversations are never assumed interchangeable.
5. Human sign-off for side effects/unsafe policy change and key workflow gates; fail closed when permissions cannot be enforced.
6. Learning is evidence-based and human-approved; model weights and autonomous self-modification are out of scope.

## Recommended grill-with-docs order
Review Phase 1 first, including adapter permission and billing capabilities; then handoff schema/context fidelity in Phase 2; then evaluation/learning governance in Phase 3. For each phase, produce: ambiguous requirements, missing scenarios, contradictions, deferred decisions, dependencies, risk register, and proposed edits. Do not implement until its exit gate is accepted.

## Status tracking
- Phase 1: Draft, not reviewed
- Phase 2: Draft, not reviewed
- Phase 3: Draft, not reviewed

[Original consolidated backlog](../feature-spec.md) remains historical reference. **Phase documents govern implementation scope after review.**
