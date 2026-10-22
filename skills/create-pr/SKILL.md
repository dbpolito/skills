---
name: create-pr
description: Prepare a branch and create a pull request with a concise title and body. Use when asked to open a PR.
---

# Create PR

- Resolve and fetch the base; ensure the branch is up to date with it before opening the PR, merging it if needed. Create a feature branch if needed, commit task-related changes, and push without asking for confirmation. Leave unrelated changes untouched; stop if there is no diff against the base.
- Read the commits, relevant diff, and any PR template; describe the net changes between the base and HEAD, not intermediate steps.
- Write a specific, short title and a compact body explaining the purpose and reviewer-relevant changes. Prefer bullets, small code snippets, or Mermaid diagrams over long prose. Don't list files or implementation trivia.
- For visual changes, include a before/after table with images or video links when available. For benchmarks, include baseline and candidate results in a table; don't invent measurements or media.
- Attach relevant local images or videos with `gh pr create/edit --attach PATH` (repeat for multiple files). Reference local paths in the body, e.g. `![Updated UI](./screenshot.png)`, for inline placement.
- Link a related ticket when provided or clearly identifiable from the branch or commits; don't guess or create one. Mention risks, migrations, or unusual validation only when relevant. Don't add a routine test report, generic checklist, or unverified claims just to fill space.
- Check for an existing PR before creating one. Push the branch if needed, create the PR against the resolved base, apply requested labels, and return its URL and title. Don't create tickets. For copy-only requests, return the title and ready-to-paste body without changing Git state or publishing.
