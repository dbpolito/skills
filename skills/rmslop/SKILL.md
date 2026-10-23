---
name: rmslop
description: Remove AI code slop
---

Remove AI-generated slop from the requested changes. Default to uncommitted changes when present, otherwise the branch's changes against its base. Keep cleanup scoped and preserve unrelated work.

This includes:

- Extra comments that a human wouldn't add or is inconsistent with the rest of the file
- Extra defensive checks or try/catch blocks that are abnormal for that area of the codebase (especially if called by trusted / validated codepaths)
- Casts to any to get around type issues
- Any other style that is inconsistent with the file
- Unnecessary emoji usage

Report at the end with only a 1-3 sentence summary of what you changed
