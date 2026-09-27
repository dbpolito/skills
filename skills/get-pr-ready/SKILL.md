---
name: Get PR Ready
description: Get an existing PR ready by fixing CI failures and addressing review feedback until checks pass and review is approved. Use when asked to get a PR ready.
---

# Get PR ready

Use the given PR, or the current branch's PR. Follow repository instructions and preserve local work.

1. Check CI and all current review feedback with `gh`. Wait for pending checks (`gh pr checks <number> --watch`); inspect failures.
2. Verify feedback against the code. Fix valid issues, test, commit, and push; politely explain disagreements or ask for clarification. Honor any required approval before guarded operations.
3. After each push, recheck CI and feedback on the latest head. Finish only when required checks pass, review is approved, and no actionable feedback remains.

If blocked by a failing check, pending review, or needed human decision, report the current state and blocker. Never self-approve, dismiss reviews, or merge to force completion.
