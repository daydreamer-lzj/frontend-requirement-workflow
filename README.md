# Frontend Requirement Workflow

An Agent Skill that turns a frontend product requirement and optional Lanhu design into a focused, verified implementation.

It follows one short workflow:

`requirement -> CodeGraph -> clarify -> spec approval -> optional Lanhu design -> implement -> verify`

## What is included

This repository publishes only the `frontend-requirement-workflow` Skill and the minimal Codex and Claude Code plugin metadata needed to distribute it.

CodeGraph and Lanhu MCP are optional external integrations. Their source code is not bundled here.

## Install in Codex

Ask Codex to install the Skill from GitHub:

```text
Use $skill-installer to install frontend-requirement-workflow from
https://github.com/daydreamer-lzj/frontend-requirement-workflow/tree/main/skills/frontend-requirement-workflow
```

The Skill is normally available from the next turn. Invoke it explicitly with:

```text
$frontend-requirement-workflow Build this frontend requirement.
```

Codex can also select it automatically when a request asks to implement a frontend page or feature from a requirement document or design.

## Install in Claude Code

Clone this repository, then copy or link the Skill into the personal skills directory:

```bash
git clone https://github.com/daydreamer-lzj/frontend-requirement-workflow.git
mkdir -p ~/.claude/skills
cp -R frontend-requirement-workflow/skills/frontend-requirement-workflow ~/.claude/skills/
```

Invoke it with:

```text
/frontend-requirement-workflow Build this frontend requirement.
```

## Optional integrations

- [CodeGraph](https://github.com/colbymchenry/codegraph) is the preferred code-understanding path. The Skill asks before initializing a repository index.
- [Lanhu MCP](https://github.com/dsphper/lanhu-mcp) is required only when the requirement contains a Lanhu design link.

When an integration is unavailable, the Skill reports the missing capability and requests authorization before installation or configuration. It does not silently modify global settings.

## Safety and scope

- Product code is not changed until the generated spec is approved.
- Business behaviour comes from the approved spec; visuals and assets come from Lanhu when present.
- The workflow avoids speculative abstractions, unrelated refactors, and unnecessary dependencies.
- It performs focused verification and a lightweight readability review.
- It does not commit or push unless explicitly requested.

## License

MIT
