---
name: get-pr-ready
description: Fix CI failures and address review feedback on an existing PR. Use when asked to get a PR ready for merge.
---

# Get PR ready

Use the given PR or the current branch's PR. Follow repository instructions and preserve local work.

1. Confirm the PR's head branch. Use `gh` to inspect failed checks, reviews, and inline review threads.
2. Verify feedback against the code. Fix CI failures and valid review issues, run relevant tests, commit only your changes, and push. Explain disagreements or ask for clarification.
3. Recheck the latest head after each push. Address available issues before waiting for required checks (`gh pr checks <number> --required --watch --fail-fast`).
4. Repeat until required checks pass, required reviews are approved, and no actionable feedback remains.

If progress requires human input or an external fix, report the current state and blocker. Never self-approve, dismiss reviews, or merge.
