---
name: rmslop
description: Remove AI code slop
---

Remove AI-generated slop from the requested changes without changing behavior. Default to uncommitted changes when present, otherwise the branch's changes against its base. Keep cleanup scoped and preserve unrelated work.

This includes:

- Extra comments that a human wouldn't add or is inconsistent with the rest of the file
- Redundant defensive checks or try/catch blocks. Trace callers, validation, and failure paths before removing them. Keep a guard or handler if it enforces a boundary, supports recovery, or you cannot establish that it is redundant.
- Casts to any to get around type issues
- Any other style that is inconsistent with the file
- Unnecessary emoji usage

Inspect the final diff for behavior changes. Report at the end with only a 1–3 sentence summary of what you changed.
