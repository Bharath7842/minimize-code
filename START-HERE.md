# Minimize Code — complete download

The package includes the same current skill for Claude Code, Codex, and Cursor. It supports audits, planning, implementation, and simplification. Claude audits start with CLAUDE.md and follow the relevant project references.

## Install the files for your tool

Work from your existing project root. Merge instruction snippets into existing files so your project rules and specifications remain intact.

| Tool | Files to copy from this package | Instructions to merge |
| --- | --- | --- |
| Claude Code | Copy `claude-code/.claude/skills/minimize-code/` into your project's `.claude/skills/` | Append `claude-code/CLAUDE.md` to your project-root `CLAUDE.md` |
| Codex | Copy `codex/.agents/skills/minimize-code/` into your project's `.agents/skills/` | Append `codex/AGENTS.md` to your project-root `AGENTS.md` |
| Cursor | Copy `cursor/.cursor/skills/minimize-code/` into your project's `.cursor/skills/`, and the rule into `.cursor/rules/` | Merge the supplied `minimize-code.mdc` if that filename already exists |

The `.claude`, `.agents`, and `.cursor` folders may be hidden in your file manager. Copy the contents for your chosen tool. For a repository used with several tools, the full guide explains how to share `.agents/skills/` and import it from Claude.

`shared/minimize-code/` contains the canonical portable skill and optional Codex UI metadata. All tool-specific SKILL.md copies are identical.

For personal installations, use the home-folder paths in `Claude-Code-Setup.md`.

## Audit with Claude Code

After installing the updated skill, send:

```text
/minimize-code audit this repository.
Read CLAUDE.md first, then follow its references to relevant README sections and current specs.
Create CODE_MINIMIZATION_AUDIT.md with evidence-backed unnecessary-code findings, proposed simplifications, required behavior to retain, risks, and proposed checks.
Change only the report file. Preserve source, tests, dependencies, and configuration.
```

Supply a real base commit or PR when you want to identify additions from a particular change. Otherwise, review current complexity without attributing its origin. Use a mode that permits writing the report; Plan Mode returns the report through the permitted chat or plan surface when repository writes are blocked.

## Plan with Claude Code

Start `claude --permission-mode plan`, then send:

```text
/minimize-code Plan the feature described in the specification path I provide.
Read CLAUDE.md, the relevant README sections, the spec, and affected code and tests.
Show requirements with sources, reuse decisions, the smallest complete design, affected files and callers, and proposed verification.
Plan only; do not edit source or install dependencies.
```

The optional `claude-code/optional/plan-default.settings.json` contains keys you may merge into `.claude/settings.json` to start terminal sessions in Plan Mode. Preserve other settings. Extension-specific setup is covered in the full guide.

The complete installation instructions, cross-tool prompts, workflow, and report requirements are in `Claude-Code-Setup.md`.
