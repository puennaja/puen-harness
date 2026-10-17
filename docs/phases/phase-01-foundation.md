# Phase 1 — Terminal Foundation & Model Selection

> Status: Draft for `grill-with-docs`. Specification only; not authorization to implement. Prioritize observable acceptance tests and raise open questions rather than assuming answers.

## Goal
An operational terminal shell to explicitly select Codex/Claude/Ollama providers and their compatible models, or use a simple explainable Auto policy, with safe execution and auditable cost/usage. Deliver **portable task context v0** before any cross-provider orchestration.

## Scope / feature IDs
- F01 Terminal chat and slash commands: `/provider`, `/model`, `/auto`, `/status`, `/cancel`, `/budget`, `/context`. Stream progress/errors; keyboard-safe cancel.
- F02 Provider adapter contract, capability probing, subprocess cleanup and typed event normalization for Codex CLI, Claude Code and Ollama. Document missing capabilities; adapter conformance tests.
- F03 Provider/model discovery: installed/authenticated models and version-specific support; manual pinning must fail clearly if unavailable.
- F04 Deterministic task-aware routing v0: select **eligible provider first, model second**, based on task kind, quality floor, privacy, permissions, context, available capabilities, cost/latency. Modes: manual, balanced, cost-first, quality-first, local-only. Show decision rationale; no silent fallback.
- F05 Safety policy: read-only default where verifiably supported; human approval before writes/side effects; fail closed if unable to enforce intended permissions. Provider-native safety/sandbox limitations documented.
- F06 Scoped sessions and durable minimal run state: working directory, task identity, native session reference, cancellation/interrupt, resume only where supported. No session format assumptions across providers.
- F07 Usage + budget visibility: measured vs estimated vs unknown cost; spending/attempt caps; subscription and API quota uncertainty explicitly labeled.
- **F08a Portable Task Context v0 (moved forward)**: schema + file-backed task pack with goal, scope, acceptance criteria, hard constraints, source refs, decisions, unresolved questions, context budget and provenance. Inspect/validate before calling agent; not a vector store.

## Deliverables
Terminal CLI, provider interface and three adapter implementations; routing policy/config and diagnostics; task-context JSON/Markdown schema + generator/reader; redacted run journal; automated smoke/adapter tests and setup docs.

## Definition of done — observable scenarios
1. On clean setup, diagnostics explain absent CLI/auth or Ollama and recovery, without crashing.
2. Operator pins a specific installed provider/model and runs read-only task; chosen model is displayed; unsupported model errors rather than switching silently.
3. Auto route shows task classification, excluded candidates, chosen provider then model and rule rationale. Restricted data never sent to ineligible remote provider.
4. Cost-first cannot override defined quality/permission minimums; unknown price is NOT treated as free. Caps prevent starting further runs.
5. User cancels mid-stream; subprocess/stream terminates and journal marks incomplete, not successful.
6. An attempted unsafe write without adequate approval/enforcement is blocked; escalation requires explicit user action.
7. Task pack persists acceptance criteria and source refs across CLI restarts, has provenance and scoped working directory.
8. Run record captures model, provider, version, route reason, outcomes, usage availability, and errors with credential redaction.

## Non-goals
Automatic provider handoff; semantic retrieval; adaptive compaction; multi-agent loop; long-term learning; automatic PR/deploy; silent config mutation.

## Dependencies & risks
Adapter capabilities may differ by CLI version and auth mode; provider subscription/API costs are not directly comparable. Node/TypeScript + Ink is proposed, not yet a locked choice. Unknown permission enforcement must fail closed.

## Grill-with-docs questions
- Does CLI integration expose the exact requested model and reported usage for all chosen provider versions?
- Which operations can be enforced at process boundary vs require provider-native approval hooks?
- What is the fallback behavior if candidate cost/quality/context metadata is unknown?
- Which fields in Task Context v0 are required, immutable, or sensitive?
- Can every acceptance case be demonstrated on each adapter, or is it capability-gated with an explicit limitation?

## Exit gate
Human sign-off on adapter conformance, manual+Auto routing, permission enforcement and portable task context contract; record unresolved limitations explicitly.
