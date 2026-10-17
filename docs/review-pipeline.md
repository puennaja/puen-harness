# Puen Harness — PR Review Pipeline v0 (pilot)

Status: proposed on feature branch. No production runtime changes; no merge authorization.

## Goal
Codex develops on a feature branch, pushes and opens a Draft PR. When the author marks it Ready for Review (or subsequently pushes updates), GitHub Actions performs read-only independent AI review, posts an advisory PR comment, and the human gate owner decides whether to approve/merge.

## Current design
- Uses the official `openai/codex-action@v1` with `:read-only` permission profile. This is **a new AI reviewer instance, not the ChatGPT conversation named Wit**.
- Runs only on non-draft, same-repository PRs authored by `puennaja`. Fork PRs are excluded to reduce untrusted-code/secret exposure.
- Workflow has read-only repository token during model review; a separate job receives narrowly scoped PR-comment permission.
- AI does not approve, edit, execute tests, or merge; it reviews source/diff and existing test evidence.
- No review triggers while Draft. Mark Ready for Review to start. Pushing after that retriggers; outputs may produce multiple comments. Concurrent runs for the same PR are canceled when superseded.
- Human approval is **not automatically enforced by this workflow**. Configure GitHub branch protection/rulesets separately; verify restrictions apply to admins/owner under actual plan.

## Setup required by human
1. Review and merge this setup PR (first activation is bootstrapping).
2. Create an OpenAI API key with a limited budget, and set it as repository **Actions secret** `OPENAI_API_KEY` under Settings → Secrets and variables → Actions. Do not commit or paste credentials.
3. Configure branch protection for `master`: require pull requests and desired human approving review/checks; prevent bypass if available. A bot PR comment does **not** count as a human approval.
4. Open a small test Draft PR, mark Ready for Review, confirm AI comment and workflow timing/spend; inspect unexpected/duplicate runs. Review failures should be visible, never treated as approval.
5. Set model and pinned action SHAs after first validated version/price choice, before treating as a strong release control. Current `@v1` and other tags are moving tags and are **not supply-chain pinned**.

## Budget and risks
- GitHub Actions review uses **OpenAI API billing**, separately from ChatGPT Plus/Codex subscriptions. The operator must control API key budget and review triggers; no guarantee of strict per-run monetary ceiling.
- Public PRs and repo files are potentially attacker-controlled prompts; reviewer must not act on embedded instructions. Constraining authors alone does not mean contents are trusted.
- `pull_request` runs with a secret only for same-repository trusted-author PRs; avoid `pull_request_target` with untrusted checkout. Checkout has `persist-credentials: false`.
- Review never runs tests or code by instruction; checkout/action setup executes trusted GitHub Actions code. Auditing/pinning third-party action dependencies remains necessary.
- Only a human should determine acceptance; AI review is advisory and may miss defects or make unsupported claims.
- The manually configured GitHub branch protection must be validated. This PR does not install or claim owner-only gate enforcement.
- Initial automated reviewer is Codex, not Claude. Cross-model review remains a future replaceable adapter; do not confuse this with Puen Harness's coding-provider adapters.

## Review coverage
Review current diff against Phase 1 approved requirements, security boundaries, task/run/session lifecycle, context authority, provenance, failure handling, tests and regression risk.

## Initial smoke acceptance
- Draft PR creates **no** AI review.
- Ready for Review PR authored by `puennaja` triggers AI review if API secret exists.
- Missing secret produces explicit workflow failure and no success comment.
- New commit to ready PR retriggers and labels head SHA; prior comments do not approve the newest commit.
- Reviewer cannot modify repo or post an approval/merge; human retains final responsibility.
