# Get PR ready

Ask “get this PR ready” with a PR number or URL; otherwise use the current branch's PR. Requires authenticated `gh` access to the repository.

## Behavior

Confirm the repository and head branch, preserve local work, and inspect failed checks, reviews, and all inline threads, including replies and outdated comments. Paginate all collections.

Fetch the latest base from the base repository. Merge it when its tip is not an ancestor of the PR head, even without reported conflicts. Respect repository history policy and do not rewrite published history without authorization.

Verify feedback against the code. Fix CI failures and valid issues across affected consumers and related paths; run relevant tests at actual entry points or enforcement boundaries. Commit only the skill's changes and push cohesive fixes.

After successful pushes, reply in each addressed inline thread with the change, pushed commit link, and accurate validation results. Also explain already-present fixes lacking adequate replies. Read existing replies, avoid duplicates, and refetch after uncertain submissions. Delivery failures remain blockers.

Recheck the latest base/head, checks, and feedback. Address available issues before waiting for required checks and already-triggered review automation, including advisory automation. A successful automation job does not establish that it published a complete review.

## Ready state and limits

The PR is ready only when its head includes the freshly fetched base tip, required checks pass, required reviews are approved, and no actionable feedback remains. Report human or external blockers rather than claiming readiness.

Disagreements need evidence in the original thread. Accepted or deferred risks require an explicit user decision. Leave resolution to reviewers unless requested; never resolve an unfixed concern, self-approve, dismiss reviews, or merge the PR.

## Evidence

Static observation: `skills/get-pr-ready/SKILL.md`. The manifest specifies numeric root-comment IDs for REST replies and safe JSON construction with `jq`. No feedback replies, tests, merges, or pushes were executed for this baseline.
