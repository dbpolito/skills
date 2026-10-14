# Initialize project documentation

Initialization is part of specify. A first invocation such as “@doc-driven” is enough when no product-documentation baseline exists. Build the initial docs, set up their viewer, and stop for review; application behavior remains unchanged.

1. Understand the project from its README, manifests, entry points, user-facing surfaces, implementation, and tests. Establish what it does, who uses it, its main areas, and how users interact with it. For a skill collection, read the skill manifests and supporting references; for an application, trace a representative user flow through code and tests. If the repository cannot establish product purpose, ask rather than invent it.
2. Summarize that understanding and announce the smallest useful structure before writing. Build an overview of the whole project and a detailed baseline for one representative area by default. For a small collection of independent skills, one concise page per skill is a practical initial version. Respect a narrower requested scope. Treat existing draft docs as evidence to reconcile, not as established requirements, and preserve unrelated documentation.
3. Organize docs around the product's actual areas. For a skill collection, use an Overview at `docs/index.md` and `docs/skills/<name>.md` explaining each requested skill's usage, behavior, and limits. For an application, default to this structure, creating only files that the slice needs:

   ```text
   docs/
     index.md
     pages/<surface>.md
     features/<behavior>.md
     rules/<shared-rule>.md
   ```

   The index states product purpose, links documented areas, and makes coverage limits clear. Pages own layout, interactions, and page-specific states. Features own reusable functionality. Rules own cross-cutting permissions, validation, and formatting. A behavior used on only one page can stay there until reuse justifies extraction.
4. Write the baseline with concise evidence paths. Label static observations as observations; identify test-backed expectations and user-confirmed intent separately. Record uncertain behavior and possible bugs as open questions. Do not manufacture loading/error states, visual measurements, or rules absent from the evidence. Extract shared docs only when actual reuse warrants them. Add concrete acceptance examples where useful and state whether they were executed.
5. Initial setup documents existing behavior; it does not authorize product changes. Later changes update these docs directly for review, following `SKILL.md`.
6. Set up and validate the docmd viewer below. Finish only when the overview links the documented areas, shared references resolve, coverage limits and uncertainty are visible, generated output is ignored, and the final diff contains only documentation and necessary viewer setup. Report checks and gaps, give the viewing command, and **stop for user review**.

## docmd as the viewer

Keep ordinary Markdown with relative `.md` links; use Mermaid fences only where a diagram helps. Cite implementation outside the docs tree with code-formatted paths or repository URLs.

1. Preserve existing viewer setup and preferences. For a new viewer, keep config, generated output, runtime dependencies, and ignore rules inside `docs/`. Create `docs/docmd.config.json` using this base, with the AI Assistant disabled and Search/Mermaid enabled. Replace the title with the product's name and adapt paths to the actual docs. Exclude generated output and dependencies from source scanning so repeated builds and validation do not rescan them.

   ```json
   {
     "title": "Project name",
      "src": ".",
      "out": "./.docmd-site",
      "plugins": {
        "ai": false,
        "search": true,
       "mermaid": true
     }
   }
   ```

2. Add `navigation` matching the actual docs, with Overview (`path: "/"`) first. For groups, use `title`, `children`, and `collapsible: false` so links are visible on the landing page. This means always expanded, not initially open with a collapse toggle. Keep navigation in sync when adding or moving docs; preserve established project preferences.
3. Create `docs/.gitignore` with `/node_modules/` and `/.docmd-site/`, adding other viewer cache paths only when needed. Run docmd with `docs/` as the process working directory: `--cwd docs` alone may still install runtime dependencies at the repository root. Existing ancestor dependencies may still be resolved; preserve them and report any resulting conflicts. Check the installed version's schema and plugin-load warnings: Search and Mermaid ship with 0.9.7, but a successful build can still omit a failed plugin. Start without a package manifest, custom CLI, or extra plugins. If bundled dependencies fail to load through `npx`, report the blocker; offer a local `@docmd/core` dev dependency inside `docs/` rather than silently declaring success or changing project dependencies.
4. Run `npx @docmd/core build` and `npx @docmd/core validate` with `docs/` as the working directory. Repeat the build to confirm generated output and dependencies are excluded. For a new viewer, confirm generated HTML omits the Ask AI Assistant widget and AI client assets. When practical, smoke-test `npx @docmd/core dev` there in the foreground. With browser authorization, check the project title, visible navigation groups, and a real search result; render a small Mermaid flowchart in a disposable fixture. Report untested behavior, including live reload, and keep server lifetime under the user's control.
5. Include `(cd docs && npx @docmd/core dev)` in the overview, README, and final response as the viewing command from the repository root. Adapt the directory for projects using a different docs location. Finish with Markdown sources and viewer setup, not generated output.
