---
name: frontend-requirement-workflow
description: Run frontend feature work from a product requirement and optional Lanhu design through one-question clarification, an approved Markdown spec, CodeGraph-guided implementation, and focused verification. Use when the user asks to build a frontend page or feature from a requirement document or design. Do not use for isolated bug fixes, tiny styling edits, explanation-only requests, or standalone code review.
---

# Frontend Requirement Workflow

Turn a requirement into a verified frontend change through one short path:

`requirement -> CodeGraph -> clarify -> spec approval -> optional Lanhu design -> implement -> verify`

Keep the workflow proportional to the feature. Do not introduce tickets, a workflow state machine, planning artifacts, or extra abstractions unless the user asks for them.

## 1. Establish the project context

Before implementation:

- Read the applicable global, repository, and nearer-directory `AGENTS.md` files. Follow their precedence rather than copying their rules into the spec.
- Inspect the package manifest, lockfile, build configuration, CI, and relevant documentation to identify the framework, package manager, established patterns, target runtime, and available verification commands.
- Inspect `git status` and preserve existing user changes. Do not alter unrelated work.
- Fully read the supplied requirement source. Accept pasted text, local Markdown/text/Word/PDF files, Wiki or Confluence pages, and ordinary web pages through the appropriate available reader.
- Record links or paths to the original requirement in the spec. Do not treat a partial read as complete.

When a required reader or capability is unavailable, handle it just in time:

1. If a missing Skill has a known curated name or GitHub source, ask for authorization and use the Skill installer.
2. A newly installed Skill is normally available on the next turn. Stop at the current phase and tell the user how to resume.
3. If an MCP or CLI is missing, request authorization to install or configure it only when its source and configuration are known.
4. Never guess configuration, silently edit global settings, or fabricate unavailable source content.

## 2. Use CodeGraph

CodeGraph is the default code-understanding path.

- If `.codegraph/` exists, check its status and run `codegraph sync <repo>` only when the index is behind. Use `codegraph explore` or the equivalent MCP tool before grep/find or broad file reading.
- If `.codegraph/` is absent, explain that initialization creates a repository index and request confirmation before running `codegraph init <repo>`.
- Do not run a full `codegraph index` unless the index is demonstrably broken and the user approves rebuilding it.
- If CodeGraph fails, report the failure and ask whether this run may fall back to ordinary code search. Do not downgrade silently.

Use the requirement's domain terms to locate existing routes, pages, components, state, requests, types, assets, and tests. Load only the code needed to understand the affected path.

## 3. Clarify and write the spec

Resolve product decisions by interviewing the user one question at a time:

- Attach a recommended answer to every question.
- Discover repository facts yourself instead of asking the user.
- Separate confirmed requirements from implementation assumptions.
- Continue until there are no material open questions about scope, behaviour, states, or acceptance.

Write one ordinary Markdown spec. Follow the repository's declared spec location; otherwise use `docs/specs/<feature-name>.md`. Do not add workflow status metadata.

Keep the spec concise but sufficient for implementation. Include:

- original requirement sources and any Lanhu link;
- goal and in-scope user behaviour;
- relevant pages, entry points, interactions, and UI states;
- data/API dependencies and known constraints;
- concrete acceptance criteria;
- explicit out-of-scope items and unresolved external dependencies.

Do not include guessed behaviour, detailed code plans, stale file paths, or raw design-tool data.

Present the completed spec and wait for explicit approval before changing product code. If a later session cannot establish that the spec was approved, ask the user; do not invent persistence metadata.

## 4. Read the design when present

Lanhu is optional. When the requirement or spec includes a Lanhu link:

- Require the Lanhu MCP and verify the intended project, page, artboard, and available version before implementation.
- Extract only implementation-relevant layout, dimensions, typography, colors, spacing, radii, assets, responsive behaviour, interactions, and visible states.
- Treat the approved spec as the source of truth for business behaviour and Lanhu as the source of truth for visuals and assets.
- If they conflict, stop and ask the user. Do not silently choose one.
- Do not infer permissions, API semantics, error handling, or other business rules from visual appearance alone.
- Reuse repository assets first. Download only assets used by the approved scope, follow the project's asset conventions, and do not store raw Lanhu analysis data.
- If a required design or asset cannot be read, report the gap rather than substituting a lookalike or claiming fidelity.

When no Lanhu link is supplied, implement from the approved spec and the project's existing design system. Do not block solely because Lanhu is absent.

## 5. Implement the approved scope

Before editing, use CodeGraph to refine the affected path and identify reuse opportunities. Then implement the smallest coherent change that satisfies the spec and design.

Follow these readability constraints in addition to project rules:

- Every changed line must trace to an acceptance criterion or a necessary integration step.
- Keep one user action or business flow locally coherent; do not scatter it across unrelated modules.
- Do not extract single-use logic that remains clearer inline.
- Do not add pass-through helpers, wrapper components, speculative configuration, or hypothetical extension points.
- Extract only for demonstrated reuse, genuinely complex computation, a stable interface, or a useful test seam.
- Prefer clear domain names and straightforward control flow over clever compression.
- Reuse existing architecture, components, request patterns, state management, styles, and dependencies.
- Do not refactor adjacent code, upgrade the stack, add dependencies, or modify generated files unless the approved requirement needs it.
- Keep external inputs read-only, avoid duplicated derived state, clean up side effects, handle stale async results where relevant, and avoid unjustified `any` or type assertions.

## 6. Verify and hand off

Choose verification from the repository's own scripts and the actual risk of the change:

- Run focused syntax, lint, type, and affected tests for edited code.
- Run broader checks only when the user requests them, submission is imminent, or the change affects shared types, dependencies, build configuration, or similarly global surfaces.
- When runnable UI and browser tooling are available, exercise the affected route at representative target viewports and compare the implemented states with Lanhu.
- Distinguish code verification from target-device or human visual acceptance. Never claim visual completion without inspecting the rendered page.

Finish with a lightweight review of the current diff. Correct issues introduced by the change involving over-abstraction, needless indirection, unclear naming, fragmented business logic, duplicated state, scope creep, or violations of applicable project rules. Do not expand this into unrelated cleanup or a full code-review workflow.

Report:

- what was implemented;
- the spec and design sources used;
- checks actually run and their results;
- any unresolved dependency or required human/device validation.

Do not commit or push unless the user explicitly requests it. Before a requested commit, run the repository's required submission checks.
