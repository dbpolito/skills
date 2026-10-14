---
name: doc-driven
description: Initialize concise product docs with docmd, edit docs for review, and implement approved documentation changes.
---

# Documentation-driven development

Update concise product docs first, review the documentation diff, then implement the approved behavior. The docs are the specification; separate specs, issues, and implementation plans are unnecessary.

## Start here

1. Read repository instructions, README, structure, and existing docs. Product docs need an overview linked to the product's actual areas; a README alone or generated output is insufficient.
2. For initial docs, or an invocation without a stage when no product docs exist, follow [setup.md](setup.md). Document existing behavior and stop for review; leave implementation unchanged.
3. Otherwise use **specify** or **implement** as requested. If the stage or area is unclear, ask. Reuse existing docs.
4. Implementation requires explicit approval of the relevant documentation changes and an explicit implement request. Missing docs or approval are blockers.

## Find the scope

Read the index, affected docs, and linked shared rules. Follow relevant references without rereading cycles, and search incoming links and implementation consumers. Clarify uncertainties affecting behavior, permissions, or acceptance before treating them as requirements.

## Documentation convention

- Describe intended product behavior directly: layout, interactions, states, and rules, not code-edit recipes. Add concrete examples when they resolve ambiguity. The documentation diff is the review artifact; Git preserves previous behavior.
- Define shared behavior once and link consumers with relative Markdown links. Keep scope and exceptions with the owning rule. Create shared docs only when behavior is reused.
- Keep docs concise. Use ordinary sections rather than proposal sections or status labels.
- Keep uncertainty in `## Open questions` and known implementation or verification gaps in `## Implementation gaps`. Distinguish observed behavior from intended behavior with concise evidence paths. Target behavior in the docs is not proof that it is implemented.

## Specify — documentation only

1. Inspect relevant implementation and tests as evidence. Report discrepancies without assuming code or docs are correct. In skill repositories, manifests are implementation; reading them does not prove runtime compliance.
2. Edit the affected docs directly to describe the desired behavior. Update shared rules and consumer links together. Record known differences from implementation as gaps.
3. Check links and the diff: affected behavior is documented or explicitly unresolved, shared rules have one owner, and implementation/tests are unchanged. Existing executable skill instructions, including `SKILL.md`, are implementation. Viewer setup is allowed during initialization or when requested.
4. Report the documentation changes, evidence, questions, and gaps. **Stop for user review.** Approval alone does not start implementation.

## Implement — approved requirements

1. Identify the approved docs and scope from the user's decision and relevant diff or revision. Changed requirements need renewed approval. If approval is missing or ambiguous, ask; stop on questions affecting implementation. No approval registry or in-document status is required.
2. Read the approved docs, shared rules, and affected consumers. Update implementation and relevant tests within that scope, following repository patterns. Recheck shared behavior at its consumers.
3. Verify against the documented behavior using appropriate tests or explicit manual/prompt scenarios. Report actual results and checks not run. For skills, distinguish static review from observed agent execution; external side effects require authorization or a safe sandbox.
4. Seek a user decision when requirements cannot be met or need changing; keep approved behavior intact rather than redefining success.
5. Remove implementation gaps only after completion and verification. Keep incomplete or untested behavior visible. Finish with implemented scope, checks, and remaining gaps.
