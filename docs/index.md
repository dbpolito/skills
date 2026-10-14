# Skills

Small, focused skills for coding agents working on pull requests, isolated checkouts, code cleanup, and documentation-driven development.

## Install and use

Install the collection with the [skills CLI](https://skills.sh/):

```sh
npx skills add dbpolito/skills
```

Select one skill with `--skill <name>`. Add `-g -a opencode` for a global OpenCode installation. Each page below describes when to invoke that skill and what changes or external actions its manifest permits.

## Areas

| Skill | Purpose |
| --- | --- |
| [Review PR](skills/review-pr.md) | Investigate pinned changes and publish a formal GitHub review. |
| [Get PR ready](skills/get-pr-ready.md) | Fix checks and feedback without merging the PR. |
| [Create PR](skills/create-pr.md) | Prepare and publish a PR, or draft its text only. |
| [Worktree](skills/worktree.md) | Create an isolated checkout and move the current session. |
| [Rift](skills/rift.md) | Legacy Rift-specific workspace creation. |
| [Remove slop](skills/rmslop.md) | Clean up code introduced by a branch. |
| [Documentation-driven development](skills/doc-driven.md) | Review product docs before implementing approved behavior. |

## Evidence and coverage

This baseline describes static observations from `skills/*/SKILL.md`, `skills/doc-driven/setup.md`, `README.md`, and the two workflow files under `examples/`. It covers all seven skill manifests and the review automation examples. Manifests prescribe agent behavior; reading them does not establish that an agent follows them at runtime.

Skill-specific uncertainties are recorded on their owning pages. There is no shared permission policy here: review, publication, cleanup, and workspace creation have different action boundaries.

## Browse locally

From the repository root:

```sh
(cd docs && npx @docmd/core dev)
```

The docmd viewer is configured for visible skill navigation, Search, and Mermaid support, with the AI Assistant disabled. Generated output and runtime dependencies stay inside `docs/` and are ignored by Git and excluded from source scanning.

## Implementation gaps

- No automated skill behavior tests were found in this checkout. Agent execution, GitHub publication, CI workflows, installation, and workspace movement were not exercised for this baseline.
- docmd 0.9.7 intermittently fails to load Search through `npx` (`@docmd/api` missing). The running development server successfully returned results for `worktree`, and following a result opened the correct page. Build reproducibility remains unresolved. A local `@docmd/core` dev dependency inside `docs/` is a possible remedy requiring a user decision, not yet tested.
- Browser checks on the running development server confirmed the Skills title, eight visible navigation links, successful responses for all eight pages, active-page navigation, and no AI Assistant widget/client. Screenshot capture was unavailable because the desktop tab was not visible; visual appearance, Mermaid rendering, and live reload remain untested. Viewer checks are separate from runtime skill verification.
