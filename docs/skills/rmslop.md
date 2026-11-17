# Remove slop

Ask “rmslop” to clean up uncommitted changes, or “rmslop this branch” for branch changes. Agents can also invoke it automatically.

## Behavior

Use the requested scope. Default to uncommitted changes when present, otherwise branch changes against the base. Preserve unrelated work.

Cleanup preserves behavior. It includes unnecessary comments, redundant defensive checks and try/catch blocks, `any` casts bypassing type issues, unnecessary emoji, and inconsistent style. Trace callers and failure paths before removing guards or handlers; keep those that enforce a boundary or support recovery, or whose redundancy cannot be established. Check the final diff for behavior changes.

Finish with only a one-to-three-sentence summary of changes.

## Evidence

Static observation: `skills/rmslop/SKILL.md`. No cleanup was applied for this baseline.
