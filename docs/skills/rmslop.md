# Remove slop

Ask “rmslop” to clean up uncommitted changes, or “rmslop this branch” for branch changes. Agents can also invoke it automatically.

## Behavior

Use the requested scope. Default to uncommitted changes when present, otherwise branch changes against the base. Preserve unrelated work.

Cleanup includes unnecessary or inconsistent comments, defensive checks and try/catch blocks abnormal for the surrounding code, `any` casts bypassing type issues, unnecessary emoji, and other inconsistent style. Surrounding conventions determine whether code is out of place.

Finish with only a one-to-three-sentence summary of changes.

## Evidence

Static observation: `skills/rmslop/SKILL.md`. No cleanup was applied for this baseline.
