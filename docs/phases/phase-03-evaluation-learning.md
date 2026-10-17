# Phase 3 — Evaluation, Learning & Adaptive Optimization

> Status: Draft for `grill-with-docs`. Specification only; not authorization to implement. Prioritize observable acceptance tests and raise open questions rather than assuming answers.

## Goal
Improve harness decisions over time using **verified outcomes**, without assuming model self-reports equal quality or enabling unsupervised self-modification.

## Dependencies
Phase 2 must emit reliable provenance, run journal, verification and cost metrics before optimization is valid.

## Scope / feature IDs
- F14 Evidence-based routing optimization: measure pass/fail, rework, acceptance-criteria coverage, cost/latency and provider reliability by task class; suggest route changes with confidence/sample count; compare to baseline. Human approves activation; simple override remains available.
- F15 Validated learning memory: scoped, versioned lessons from reviewed mistakes/fixes, decisions, conventions; source evidence, reviewer, applicability, expiration/review time, deduplication and ability to delete/disable. Retrieval must be opt-in by policy and relevant to current task.
- F16 Evaluations/regressions: representative tasks, reproducible baseline, deterministic checks where possible, blinded independent reviews for qualitative cases, context-recall tests, handoff retention and routing cost-quality tradeoff report. Reject regressions.
- F17 Policy/config UX: profiles (global/project), explicit precedence, privacy/provider allowlist, context budget, model quality minimum, spending caps, pin/override, migration and validation.

## Deliverables
Evaluation dataset schema and fixtures, local benchmarking command, routing comparison report, versioned learning entries and approval flow, policy profiles with documented precedence, rollback and audit trail.

## Definition of done — observable scenarios
1. Repeat benchmark suite on baseline policy vs proposed routing change; compare quality, actual/estimated cost, latency, sample size and missing measurements.
2. A cheaper model losing required test coverage is rejected despite savings; a beneficial routing rule stays a proposal until approved.
3. Verifier failure creates a draft lesson **not** injected into later tasks until validated and scoped.
4. Expired or contradicted lesson is excluded or flagged; delete/disable prevents use in subsequent context pack.
5. An old session's incorrect compaction causes context-recall regression and blocks rollout.
6. Config conflicting with user privacy policy or unsupported provider capability fails validation; manual model choice still requires eligibility.
7. Evaluation outputs permit reproducing provider/model versions, policy revisions, test fixture versions and outcome evidence.

## Non-goals
Fine-tuning weights, automatic codebase-wide convention inference, autonomous changes to approval/safety policy, model leaderboard claims without comparable tasks.

## Grill-with-docs questions
- Which outcome metrics reflect useful work, and what are the weighting/guardrails?
- How large must task-class samples be before proposing default changes?
- How are nondeterministic outputs and different provider pricing plans normalized?
- What criteria qualify a lesson as verified and when does it expire?
- How will evaluations resist gaming / leakage and detect regressions in context quality?

## Exit gate
Human approves evaluation methodology, learning governance and safe rollback; improvement claims supported by reproducible evidence against baseline.
