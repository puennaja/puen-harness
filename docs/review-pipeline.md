# Puen Harness — PR Review Pipeline v0.2 (subscription-first)

Status: proposal on Draft PR. No deployment, provider inference, or extra AI API.

## Goal and flow
1. Codex creates scoped feature branch, implements only approved scope, runs permitted tests, pushes and opens Draft PR to `master`.
2. GitHub Actions runs deterministic PR preflight, including branch/base convention and required information when Ready for Review.
3. User asks Wit in ChatGPT to review the GitHub PR, or uses the existing optional hourly watch when enabled. Wit reads the actual diff and test evidence using the GitHub connection, writes a **COMMENT** review with findings, and marks Ready for Review only when no blocker remains.
4. Codex addresses requested changes on the same branch within authorized scope and pushes. A new head SHA requires review again.
5. **Only the user** decides whether to merge. AI does not merge or grant a formal GitHub approval.

## What is automatic and what is not
- **Automatic:** GitHub Actions PR metadata/contract check, on opened/edited/synchronize/ready_for_review/reopened. Draft PRs can omit detailed body sections until ready.
- **Manual or scheduled ChatGPT:** Semantic/architecture/code/security review. It is **not** invoked by GitHub Actions; no API or subscription bridge is assumed.
- **Human:** approval and merge decision. A PR author may be unable to submit a GitHub approving review for their own PR; use manual merge decision in that case. GitHub branch protection's required external approving review needs another authorized GitHub reviewer account. Do not claim this workflow enforces owner-only human approval.

## Costs
- **No OpenAI/Anthropic API calls, API key, secrets, or inferred subscription usage.**
- Uses standard GitHub-hosted Actions. GitHub Actions minutes/quotas and any overages depend on repository visibility and GitHub plan; this is not guaranteed zero infrastructure cost.
- ChatGPT review is conducted through the existing conversation/connection, within applicable product usage limits. It is not continuous or real-time unless the user enables a separate scheduled check.

## Permissions and security
- Workflow has only `contents: read` and `pull-requests: read`. It does not check out or execute PR-controlled repository code.
- The workflow reads PR title/body/branch from event data as plain text. It does not interpolate these values into shell source code; they are passed as environment values.
- The job does not approve, post PR comments, write files, or merge.
- Check verifies only presence of topics (not their truth). Passing it must never be interpreted as correctness, comprehensive coverage, test success or AI approval.
- Pin exact action commit SHAs before adding third-party actions; the initial workflow uses no external actions.
- Repo owner configures appropriate branch rulesets/branch protection separately and confirms they work with account/repository plan and any admin bypass.

## Human review policy
- Compare PR head SHA with reviewed SHA. If head changes, previous findings may be stale; re-review before declaring Ready.
- Report findings as BLOCKER/MAJOR/MINOR with path, reason and next steps. Explicitly distinguish tests run from tests planned/not run.
- Only remove Draft after reading actual changed files and recording a review comment. A passing metadata preflight alone is not enough.
- Keep Human Gate for merge and for any provider execution/credentials/billing/expanded implementation permission.

## Smoke test after merge
1. New Draft PR targeting master: preflight passes without section requirements.
2. Invalid branch/base: preflight fails.
3. Ready PR with absent scope/evidence/risks/human-decision topics: preflight fails.
4. Complete PR description: preflight passes (does **not** imply code quality).
5. Confirm there are no API secrets, AI actions, inferred costs, or automated merge steps.
6. Verify branch protection/manual merge constraints independently and record actual test outcomes.

## Not in v0.2
- An autonomous AI GitHub review bot, subscription credential sharing with CI, self-hosted runner on personal machines, automatic Codex repair loop, semantic code review in Actions, or auto-merge.
