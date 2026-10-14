# Documentation-driven development

Invoke `@doc-driven` to initialize a project without product docs. For existing docs, request **specify** with an area and desired behavior, or explicitly request **implement** after approving the relevant documentation changes. Ask when stage or scope is unclear.

## Initialization

Read repository instructions, README, structure, implementation, and tests to establish product purpose and actual areas. A README alone or generated output is not a product-doc baseline.

Announce the smallest useful structure before writing: an overview plus one representative area by default, or a concise page per skill for a small collection. Document existing behavior with evidence paths; do not invent requirements from missing evidence. Set up the docmd viewer and stop for review without changing application behavior or executable skill instructions.

## Specify

Read affected docs, linked shared rules, incoming references, and implementation consumers. Resolve uncertainties affecting behavior, permissions, or acceptance before treating them as requirements.

Edit product docs directly to describe layout, interactions, states, and rules rather than code-edit recipes. Define reused behavior once and link consumers with relative Markdown links. Keep uncertainty under **Open questions** and known implementation or verification differences under **Implementation gaps**. Manifests provide static evidence, not proof of runtime compliance.

Check links and the documentation diff. Implementation and tests, including `SKILL.md`, remain unchanged; initialization or requested viewer setup is allowed. Report evidence, questions, and gaps, then stop. Approval alone does not start implementation.

## Implement

Require both explicit approval of the relevant docs and an explicit implement request. Missing docs, ambiguous approval, or unresolved implementation-affecting questions block progress. Changed requirements require renewed approval.

Update implementation and relevant tests only within the approved scope, rechecking shared behavior at consumers. Verify with appropriate tests or explicit scenarios and report actual results and checks not run. External side effects need authorization or a safe sandbox. Seek a user decision when approved behavior cannot be met rather than redefining success.

Remove gaps only after completion and verification; incomplete or untested behavior stays visible.

## Viewer

Keep ordinary Markdown, relative links, config, generated output, runtime dependencies, and ignore rules inside `docs/`. New viewers use visible navigation with Overview first, expanded groups, Search/Mermaid enabled, and the AI Assistant disabled. Preserve existing viewer preferences.

Run build and validation from `docs/`, repeat the build to detect output rescanning, and inspect schema/plugin warnings. A successful build alone does not prove plugins loaded. If bundled dependencies fail through `npx`, report the blocker and offer a local dependency rather than silently adding one. Browser checks require authorization; server lifetime stays under user control.

Browse from the repository root with `(cd docs && npx @docmd/core dev)`.

## Evidence

Static observations: `skills/doc-driven/SKILL.md` and `skills/doc-driven/setup.md`. This invocation exercises initialization only; approval gating and later implementation have not been tested through agent scenarios.
