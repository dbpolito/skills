# Create PR

Ask “create a PR” and optionally provide the base branch. A request for title/body copy only returns ready-to-paste text without changing Git state or publishing.

## Behavior

- Resolve the base and stop when there is no diff against it. Create a feature branch if needed, commit task-related changes, and push without an additional confirmation; leave unrelated work untouched.
- Describe the net base-to-HEAD changes using commits, the relevant diff, and any repository PR template. Keep the title specific and short and the body focused on purpose and reviewer-relevant changes.
- Include before/after media or benchmark comparisons when available; never invent evidence. Link a supplied or clearly identifiable ticket without creating one.
- Check for an existing PR before creating one. Publish against the resolved base, apply requested labels, and return the title and URL.

Risks, migrations, and unusual validation belong in the body when relevant; routine checklists and unverified claims do not.

## Evidence

Static observation: `skills/create-pr/SKILL.md`. Unlike [review-pr](review-pr.md), this skill permits branch changes and publication. No PR was created during documentation initialization.

## Open questions

- The manifest requires checking for an existing PR but does not specify whether to reuse, update, or merely report it. That outcome is not established by this baseline.
