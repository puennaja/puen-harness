# Phase 3 — Evaluation, Learning & Adaptive Optimization

> Status: Draft, not reviewed. Only inherited Phase 1 boundary notes were approved with Q1–Q34 on 2026-10-17. Specification only; not implementation authorization.

## Goal
Improve harness decisions over time using **verified outcomes**, without assuming model self-reports equal quality or enabling unsupervised self-modification.

## Dependencies
Phase 2 must emit reliable provenance, verification and cost metrics before optimization is valid. Phase 1 already supplies minimal validated policies, admission accounting and bounded qualification fixtures.

## Inherited Phase 1 boundary
- F17a baseline user/project policy, precedence, provider/data allowlists, approval fingerprints, context limits and per-Task spending/attempt/time admission limits are mandatory in Phase 1.
- Phase 1 Auto uses reviewed task-specific static priorities and small qualification fixtures. This phase adds comparative evaluation, learned/weighted policy proposals and cost-first/quality-first strategies.
- Full configuration UX, broader profiles and migration extend the baseline; they cannot retrospectively weaken inherited safety/data requirements.
- Cross-Task daily/global spending caps and automatic retention/expiry are deferred beyond the MVP. Consider their placement in this phase's own review; they are not implicitly approved requirements of this still-draft phase.

## Scope / feature IDs
- F14 Evidence-based routing optimization: measure pass/fail, rework, acceptance-criteria coverage, cost/latency and provider reliability by task class; suggest route changes with confidence/sample count; compare to baseline. Human approves activation; eligible manual override remains available.
- F15 Validated learning memory: scoped, versioned lessons from reviewed mistakes/fixes, decisions, conventions; source evidence, reviewer, applicability, expiration/review time, deduplication and ability to delete/disable. Retrieval must be opt-in by policy and relevant to current task.
- F16 Evaluations/regressions: representative tasks, reproducible baseline, deterministic checks where possible, blinded independent reviews for qualitative cases, context-recall tests, handoff retention and routing cost-quality tradeoff report. Reject regressions.
- F17b Expanded policy/config UX: richer profiles, migration, rollback and human-approved policy changes over Phase 1's minimal precedence, privacy, capability, context and admission controls. Cost-first/quality-first or weighted policies require comparative evidence before activation.

## Deliverables
Evaluation dataset schema and fixtures, local benchmarking command, routing comparison report, versioned learning entries and approval flow, expanded policy profiles with documented precedence, rollback and audit trail.

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
- Do cross-Task daily/global admission caps and automatic retention belong here, and what are their honest enforcement/ownership limits?

## Exit gate
After separate specification approval and implementation authorization, human approves evaluation methodology, learning governance and safe rollback; improvement claims need reproducible evidence against baseline. Phase 1 documentation approval is not approval of this phase.
