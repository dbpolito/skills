---
name: rmslop
description: Remove AI code slop
metadata:
  opencode/autoinvoke: false
---

Check the head branch's diff against its base branch, and remove AI-generated slop introduced in the head branch.

Use the base branch specified by the user, otherwise the current PR's base branch when available, otherwise the repository's default branch. If the base is unclear or is the same as the head, ask the user which branch to compare against. Review changes since the merge base (`git diff <base>...HEAD`) so unrelated changes on the base branch are excluded. Keep cleanup scoped to changes introduced in the head branch.

This includes:

- Extra comments that a human wouldn't add or is inconsistent with the rest of the file
- Extra defensive checks or try/catch blocks that are abnormal for that area of the codebase (especially if called by trusted / validated codepaths)
- Casts to any to get around type issues
- Any other style that is inconsistent with the file
- Unnecessary emoji usage

Report at the end with only a 1-3 sentence summary of what you changed
