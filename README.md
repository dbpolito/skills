# Skills

Small, focused skills for coding agents. Install them with the [skills CLI](https://skills.sh/).

## Get PR Ready

Keep working on an existing pull request until its required CI checks pass, its review is approved, and no actionable feedback remains.

The skill uses `gh` to check CI and read review feedback, verifies comments against the code, fixes valid issues, tests, commits, and pushes. After each push, it checks the latest PR state again. If it needs a human decision or approval, it stops and tells you what's blocking progress. It never approves its own PR, dismisses reviews, or merges just to finish.

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
