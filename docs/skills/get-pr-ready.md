# Get PR ready

Ask “get this PR ready” with a PR number or URL; otherwise use the current branch's PR. Requires authenticated `gh` access to the repository.

## Behavior

Confirm the repository and head branch, preserve local work, and inspect failed checks, reviews, and all inline threads, including replies and outdated comments. Paginate all collections.

Fetch the latest base from the base repository. Merge it when its tip is not an ancestor of the PR head, even without reported conflicts. Respect repository history policy and do not rewrite published history without authorization.

Verify feedback against current code. Fix CI blockers, violated PR requirements, and material regressions introduced or worsened by the PR. Include sibling paths only when they share the root cause and break the same requirement or existing contract. Keep advisory feedback out of the fix loop. Get user approval before widening scope for a redesign, new contract, or product tradeoff. Test at the affected entry points or enforcement boundaries, then commit and push cohesive fixes.

After successful pushes, reply in each addressed inline thread with the change, pushed commit link, and accurate validation results. Also explain already-present fixes lacking adequate replies. Read existing replies, avoid duplicates, and refetch after uncertain submissions. Delivery failures remain blockers.

Recheck the latest base/head, checks, and feedback. Address actionable issues before waiting for required checks and already-triggered review automation, including advisory automation. Confirm a review was published for the exact revision; use its base/head marker when available rather than the movable GitHub review commit ID. Recheck new findings and unresolved blockers; carry forward settled decisions and unchanged coverage unless the diff or new evidence invalidates them.

## Ready state and limits

The PR is ready only when its head includes the freshly fetched base tip, required checks pass, required reviews are approved, and no actionable feedback or required acceptance gap remains. Report human or external blockers rather than pushing or triggering a review just to get a different verdict. Disclose advisory gaps separately.

Disagreements need evidence in the original thread. Accepted or deferred risks require an explicit user decision. Leave resolution to reviewers unless requested; never resolve an unfixed concern, self-approve, dismiss reviews, or merge the PR.

## Evidence

Static observation: `skills/get-pr-ready/SKILL.md`. The manifest specifies numeric root-comment IDs for REST replies and safe JSON construction with `jq`. No feedback replies, tests, merges, or pushes were executed for this baseline.
