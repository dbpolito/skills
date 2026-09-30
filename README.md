# Skills

Small, focused skills for coding agents. Install them with the [skills CLI](https://skills.sh/):

```sh
npx skills add dbpolito/skills
```

Skill directory and manifest names follow the skill manifest convention: lowercase letters, numbers, and hyphens (for example, `get-pr-ready`), with the directory matching the manifest's `name`.

| Skill | Purpose |
| --- | --- |
| [review-pr](#review-pr) | Investigate a PR and publish evidence-backed review findings. |
| [get-pr-ready](#get-pr-ready) | Fix CI failures and work through review feedback. |
| [create-pr](#create-pr) | Create a PR with a concise title and body from the final branch changes. |
| [rift](#rift) | Create and enter an isolated Rift-backed worktree. |
| [rmslop](#rmslop) | Remove AI-generated code slop from a branch diff. |

## review-pr

Review an existing pull request independently, then reconcile previous feedback and publish one formal GitHub review. It traces changed behavior through callers, state, and dependencies, checks potential defects against counterevidence, and reports concrete failures with concise inline comments.

It keeps a pinned checkout unchanged and reads test code without running tests. Reviews include a star grade, coverage summary, and revision details. Material gaps produce **Review incomplete**, never an approval. Existing unresolved findings and explicitly deferred risks remain visible across reruns.

### Install and run

```sh
npx skills add dbpolito/skills --skill review-pr -g -a opencode
npx opencode-ci@latest run --auto --skill review-pr 'Review and publish findings for PR #123 in owner/repo'
```

Requires `git`, authenticated `gh`, `jq`, the PR head checked out, and enough Git history to find its merge base. The publishing account needs permission to submit PR reviews.

### Run in CI

Copy these examples into `.github/workflows/`. Both OAuth and API-key authentication are supported. Adjust the commented sections, model, and secrets to your setup. See [opencode-ci](https://github.com/dbpolito/opencode-ci) for credential setup.

Reviews run for non-draft, same-repository PRs. The example includes an optional owner/member restriction as a comment. New commits cancel the previous review for that PR. Keep the checkout's explicit `ref`: GitHub otherwise checks out a synthetic merge commit, which fails the skill's pinned-head check.

Set repository variable `OPENCODE_MODEL` and, for OAuth, repository secrets `OPENCODE_CI_AUTH_JSON` and `PAT_TOKEN`. The PAT needs **Secrets: Read and write** for this repository. Enable **Allow GitHub Actions to create and approve pull requests** in repository Actions settings if the bot should approve clean reviews.

The review and keepalive workflows can overlap, and queued workflows retain their original repository secrets. Concurrent OAuth refreshes can invalidate or overwrite saved tokens; authentication failures may require reseeding the auth secret.

A green workflow means the agent process completed, not necessarily that it published a review. Check its formal review or **Review automation failure** comment for the outcome.

#### PR review

[View file](examples/opencode-review-pr.yml) · [Raw / download](https://raw.githubusercontent.com/dbpolito/skills/main/examples/opencode-review-pr.yml)

```yaml
name: opencode-review-pr

on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

permissions:
  contents: read
  pull-requests: write

jobs:
  review:
    # To restrict authors, append: && contains(fromJSON('["OWNER", "MEMBER"]'), github.event.pull_request.author_association)
    if: github.event.pull_request.draft == false && github.event.pull_request.head.repo.full_name == github.repository
    runs-on: ubuntu-latest
    timeout-minutes: 55
    concurrency:
      group: opencode-review-pr-${{ github.event.pull_request.number }}
      cancel-in-progress: true
    env:
      GH_TOKEN: ${{ github.token }}
      PR_NUMBER: ${{ github.event.pull_request.number }}
      BASE_SHA: ${{ github.event.pull_request.base.sha }}
      HEAD_SHA: ${{ github.event.pull_request.head.sha }}
      REVIEW_LOGIN: github-actions[bot]
      REVIEW_MODEL: ${{ vars.OPENCODE_MODEL }}
      REVIEW_AGENT: build
    steps:
      - uses: actions/checkout@v4
        with:
          # The skill reviews the PR head, not GitHub's synthetic merge commit.
          ref: ${{ github.event.pull_request.head.sha }}
          fetch-depth: 0
          persist-credentials: false

      - uses: actions/setup-node@v4
        with:
          node-version: '24'

      - name: Install review skill
        run: npx --yes skills add dbpolito/skills --skill review-pr -g -a opencode -y

      # Auth: keep this step and the save step below.
      # API key: remove both auth steps.
      - name: Load auth credentials
        id: auth
        env:
          OPENCODE_CI_AUTH_JSON: ${{ secrets.OPENCODE_CI_AUTH_JSON }}
        run: printf '%s' "$OPENCODE_CI_AUTH_JSON" > "$HOME/opencode-ci.auth.json"

      - name: Review
        # API key: uncomment these lines (or use your provider's variable).
        # env:
        #   OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          npx --yes opencode-ci@latest run --auto --thinking \
            --agent "$REVIEW_AGENT" --model "$REVIEW_MODEL" --timeout 2700 \
            --title "review-pr $GITHUB_RUN_ID/$GITHUB_RUN_ATTEMPT" \
            --skill review-pr \
            'Review and publish findings for the PR supplied in the environment.'

      # Auth only: persist refreshed credentials even after a failed review.
      - name: Save refreshed auth credentials
        if: always() && steps.auth.outcome == 'success'
        env:
          GH_TOKEN: ${{ secrets.PAT_TOKEN }}
          OPENCODE_CI_AUTH_JSON: ${{ secrets.OPENCODE_CI_AUTH_JSON }}
        run: |
          if ! cmp -s "$HOME/opencode-ci.auth.json" <(printf '%s' "$OPENCODE_CI_AUTH_JSON"); then
            gh secret set OPENCODE_CI_AUTH_JSON --repo "$GITHUB_REPOSITORY" < "$HOME/opencode-ci.auth.json"
          fi
```

#### Auth keepalive

[View file](examples/opencode-auth.yml) · [Raw / download](https://raw.githubusercontent.com/dbpolito/skills/main/examples/opencode-auth.yml)

```yaml
name: opencode-auth

on:
  schedule:
    - cron: '0 9 * * *' # Daily at 09:00 UTC.
  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: opencode-auth
  cancel-in-progress: false

jobs:
  refresh:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/setup-node@v4
        with:
          node-version: '24'

      - name: Load auth credentials
        id: auth
        env:
          OPENCODE_CI_AUTH_JSON: ${{ secrets.OPENCODE_CI_AUTH_JSON }}
        run: printf '%s' "$OPENCODE_CI_AUTH_JSON" > "$HOME/opencode-ci.auth.json"

      - name: Refresh auth
        env:
          OPENCODE_MODEL: ${{ vars.OPENCODE_MODEL }}
        run: |
          npx --yes opencode-ci@latest run --model "$OPENCODE_MODEL" --timeout 120 \
            'Reply only OK. Do not use tools.'

      - name: Save refreshed auth credentials
        if: always() && steps.auth.outcome == 'success'
        env:
          GH_TOKEN: ${{ secrets.PAT_TOKEN }}
          OPENCODE_CI_AUTH_JSON: ${{ secrets.OPENCODE_CI_AUTH_JSON }}
        run: |
          if ! cmp -s "$HOME/opencode-ci.auth.json" <(printf '%s' "$OPENCODE_CI_AUTH_JSON"); then
            gh secret set OPENCODE_CI_AUTH_JSON --repo "$GITHUB_REPOSITORY" < "$HOME/opencode-ci.auth.json"
          fi
```

Source: [`skills/review-pr/SKILL.md`](skills/review-pr/SKILL.md).

## get-pr-ready

Keep working on an existing pull request until its required CI checks pass, required reviews are approved, and no actionable feedback remains.

The skill uses `gh` to inspect CI and review feedback, including inline threads. It verifies feedback, fixes CI failures and valid review issues, tests, commits, and pushes, then replies in the addressed inline threads with commit links and validation results. It rechecks the latest head, addresses available issues before waiting for checks, and reports blockers requiring human input or an external fix. It leaves thread resolution to reviewers unless requested, and never approves its own PR, dismisses reviews, or merges.

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

## create-pr

Create a PR from the current branch's committed changes. Follow the repository's PR template when present, focus on the final outcome, and use before/after evidence for visual changes or benchmarks when available. A writing-only request does not push or publish anything.

### Install

```sh
npx skills add dbpolito/skills --skill create-pr
```

Ask your agent to **“create a PR”** and optionally provide the target base branch.

Source: [`skills/create-pr/SKILL.md`](skills/create-pr/SKILL.md).

## rift

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

## rmslop

Check the branch diff against `dev` and remove unnecessary comments, defensive checks, `any` casts, emoji, and other code that doesn't fit the surrounding style. Inspired by [OpenCode's `/rmslop` command](https://github.com/anomalyco/opencode/blob/dev/.opencode/command/rmslop.md).

### Install

```sh
npx skills add dbpolito/skills --skill rmslop
```

Ask your agent to **“rmslop this branch”**. This skill expects a `dev` branch to compare against.

Source: [`skills/rmslop/SKILL.md`](skills/rmslop/SKILL.md).
