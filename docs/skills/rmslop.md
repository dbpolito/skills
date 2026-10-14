# Remove slop

Ask “rmslop this branch” and optionally specify a base branch. The manifest disables OpenCode autoinvocation.

## Behavior

Choose the user-supplied base, otherwise the current PR's base, otherwise the repository default branch. Ask when the base is unclear or equals the head.

Review changes since the merge base (`git diff <base>...HEAD`). Remove only slop introduced by the head branch; unrelated changes on the base are outside scope.

Cleanup includes unnecessary or inconsistent comments, defensive checks and try/catch blocks abnormal for the surrounding code, `any` casts bypassing type issues, unnecessary emoji, and other inconsistent style. Surrounding conventions determine whether code is out of place.

Finish with only a one-to-three-sentence summary of changes.

## Evidence

Static observation: `skills/rmslop/SKILL.md`. No cleanup was applied for this baseline.
