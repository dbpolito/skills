# Skills

Small, focused skills for coding agents. Install them with the [skills CLI](https://skills.sh/).

| Skill | Purpose |
| --- | --- |
| [Review PR](#review-pr) | Investigate a PR and publish evidence-backed review findings. |
| [Get PR Ready](#get-pr-ready) | Fix CI failures and work through review feedback. |
| [Rift](#rift) | Create and enter an isolated Rift-backed worktree. |

## Review PR

Review an existing pull request independently, then reconcile previous feedback and publish one formal GitHub review. It traces changed behavior through callers, state, and dependencies, checks potential defects against counterevidence, and reports concrete failures with concise inline comments.

It keeps a pinned checkout unchanged and reads test code without running tests. Reviews include a star grade, coverage summary, and revision details. Material gaps produce **Review incomplete**, never an approval. Existing unresolved findings and explicitly deferred risks remain visible across reruns.

### Install and run

```sh
npx skills add dbpolito/skills --skill review-pr -g -a opencode
npx opencode-ci run --auto 'Use @review-pr to review PR #123 in owner/repo'
```

Requires `git`, authenticated `gh`, `jq`, the PR head checked out, and enough Git history to find its merge base. The publishing account needs permission to submit PR reviews.

### Run in CI

Copy [`examples/review-pr.yml`](examples/review-pr.yml) into the target repository's `.github/workflows/`. Set `OPENCODE_MODEL` to your provider/model ID and `OPENAI_API_KEY` to your provider credential; change the credential variable for a different provider. `OPENCODE_VARIANT` is optional. The example uses an API key and runs on non-draft, same-repository PRs.

The invocation is:

```sh
npx opencode-ci run --auto 'Use @review-pr to review and publish findings for the PR supplied in the environment.'
```

| Input | Purpose |
| --- | --- |
| `GITHUB_REPOSITORY`, `PR_NUMBER` | Target PR; otherwise resolved from the invocation or GitHub event. |
| `BASE_SHA`, `HEAD_SHA` | Immutable review revisions; otherwise resolved from the event or PR metadata. |
| `REVIEW_LOGIN` | Publishing login, including `[bot]` for a GitHub App; user tokens can use `gh api user`. |
| `REVIEW_MODEL`, `REVIEW_VARIANT`, `REVIEW_VERSION`, `REVIEW_AGENT` | Optional execution metadata for the review summary. |

Allow GitHub Actions to approve pull requests in the repository's Actions settings, or supply a GitHub App token and its bot login. The skill approves complete five-star reviews and uses `COMMENT` for findings or incomplete coverage. It does not request changes or merge. Each review is checked against current base/head SHAs immediately before publication.

If publication fails, the agent logs the reason and posts a **Review automation failure** PR comment linking the CI run. Retries reuse the same run/revision comment; superseded runs only log the skip. If GitHub cannot accept the comment, the agent reports that in the log too. The job status follows `opencode-ci`'s exit status.

Pin the skills source to a commit or release for reproducible CI. For account-auth setup, see [opencode-ci](https://github.com/dbpolito/opencode-ci).

Source: [`skills/review-pr/SKILL.md`](skills/review-pr/SKILL.md).

## Get PR Ready

Keep working on an existing pull request until its required CI checks pass, required reviews are approved, and no actionable feedback remains.

The skill uses `gh` to inspect CI and review feedback, including inline threads. It verifies feedback, fixes CI failures and valid review issues, tests, commits, and pushes, then rechecks the latest head. It addresses available issues before waiting for checks and reports blockers requiring human input or an external fix. It never approves its own PR, dismisses reviews, or merges.

### Install

```sh
npx skills add dbpolito/skills --skill get-pr-ready
```

To install it globally for OpenCode:

```sh
npx skills add dbpolito/skills --skill get-pr-ready -g -a opencode
```

### Use

Ask your agent to **“get this PR ready”** and provide a PR number or URL. If you don't provide one, the skill uses the current branch's PR. You'll need `gh` installed and authenticated for the relevant repository.

Source: [`skills/get-pr-ready/SKILL.md`](skills/get-pr-ready/SKILL.md).

## Rift

Create an isolated Rift-backed worktree, move the current OpenCode session into it, and continue the original task there. The skill avoids duplicate worktrees and does not silently substitute a manual Git worktree when Rift was requested.

### Install

```sh
npx skills add dbpolito/skills --skill rift
```

To install it globally for OpenCode:

```sh
npx skills add dbpolito/skills --skill rift -g -a opencode
```

### Use

Ask your agent to **“use a Rift workspace”** or **“work in a new worktree.”** OpenCode needs the Rift tools and session-management tool available.

Source: [`skills/rift/SKILL.md`](skills/rift/SKILL.md).
