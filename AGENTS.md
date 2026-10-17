# Communication

The user's primary language is Thai. Use Thai for user-facing responses, progress updates, and clarification questions unless the user requests another language. Preserve technical identifiers, file paths, commands, and code in their original form where appropriate.

# Development workflow

Use the GitHub PR-based workflow for this repository. The Implementation Agent develops the assigned Work Item; Wit provides independent technical review; the user is the Human Gate Owner with final approval authority.

1. Before starting a Work Item, read the approved specifications in `docs/phases/`, `CONTEXT.md`, `docs/adr/`, and the plan, work items and risk register in `docs/implementation/`. Preserve their architectural decisions, security requirements and acceptance criteria.
2. Work on one independently verifiable, explicitly assigned Work Item at a time. Fetch the latest `master` and create `feature/<work-item-id>-<short-description>` from it. Implement on that branch, never directly on `master`.
3. Identify authorization before acting. Live provider calls, provider credential access, costs, permission expansion and unapproved code changes require explicit human approval. When authorization is missing, document a bounded proposal and request approval; the Work Item assignment alone does not grant these permissions.
4. Run relevant authorized checks and record actual results as passed, failed, not run or blocked. Documentation and mock checks cannot establish native provider conformance.
5. Commit scoped changes using `[work-item-id] <type>: <description>`. With explicit publication authorization for that Work Item, push its feature branch and open a Draft PR targeting `master`.
6. Include Work Item/scope, implementation summary, architecture decisions/trade-offs, files changed, actual tests/results, acceptance coverage, limitations/risks and human questions in the PR. Report Wit review as pending until actual feedback is available.
7. Address review findings on the same branch within the authorized scope; request a human decision for scope expansion. Leave merge, closure and approval to the Human Gate Owner; do not merge, close, self-approve or mark the PR ready yourself. Approval of one Work Item does not authorize another Work Item or live execution.

For PH1-001 authorization, bounded proposals and the review handoff, read [the PH1-001 proposal](docs/implementation/ph1-001-authorization-and-proposal.md). The current assignment ends after opening its Draft PR; PH1-002 requires a separate assignment.
