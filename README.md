# Minimize Code

A portable agent skill for auditing existing code, planning changes, and writing the smallest readable implementation that satisfies the requirements.

Includes setup files for **Claude Code**, **Codex**, and **Cursor**.

## What it does

- **Audit:** inspect an existing repository and create an evidence-backed Markdown report of unnecessary complexity and possible simplifications.
- **Plan:** read project instructions, specifications, existing code, callers, and tests before proposing a focused implementation plan.
- **Implement:** reuse existing code and platform capabilities, avoid speculative abstractions, and verify the required behavior.

The workflow preserves public contracts, validation, security, error handling, accessibility, and project conventions. It optimizes maintained complexity rather than compressing formatting or shortening names.

## Quick start

Clone or download this repository, then choose the setup for your coding tool. Run your coding agent from the project you want to work on.

Copy the skill directory into that project and merge the supplied instruction snippets into existing files. Preserve the project's current instructions and specifications.

| Tool | Copy from this repository | Destination in your project | Persistent guidance |
| --- | --- | --- | --- |
| Claude Code | `claude-code/.claude/skills/minimize-code/` | `.claude/skills/minimize-code/` | Merge [the CLAUDE.md snippet](claude-code/CLAUDE.md) into project-root `CLAUDE.md` |
| Codex | `codex/.agents/skills/minimize-code/` | `.agents/skills/minimize-code/` | Merge [the AGENTS.md snippet](codex/AGENTS.md) into project-root `AGENTS.md` |
| Cursor | `cursor/.cursor/skills/minimize-code/` | `.cursor/skills/minimize-code/` | Copy or merge [the Cursor rule](cursor/.cursor/rules/minimize-code.mdc) into `.cursor/rules/minimize-code.mdc` |

Folders beginning with a dot may be hidden in your file manager. The full guide covers personal installations and a shared setup for repositories used with several tools.

## Audit an existing repository

In Claude Code, invoke the installed skill:

```text
/minimize-code audit this repository.

Read CLAUDE.md first to understand the project. Follow its references
to relevant README sections, current specs, and architecture documents.
Inspect implementation, callers, public exports, configuration, and tests.

Create CODE_MINIMIZATION_AUDIT.md containing:
- Unnecessary or duplicated code, with locations and concrete evidence.
- The smallest replacement that satisfies the requirements.
- Required behavior and public interfaces to retain.
- Uncertain findings, risks, proposed checks, and coverage limits.

Change only the report file. Preserve source, tests, dependencies,
and configuration.
```

Replace `this repository` with a specific folder when you want a narrower review. For Claude audits, `CLAUDE.md` is the first project source; the skill then reads the relevant documents it references. If that file is missing, the report states the missing source and uses the available project instructions.

Supply a real base commit, branch, or PR when you want to identify additions from a particular change. Without a relevant baseline, the audit reviews current complexity without attributing its origin.

A report distinguishes supported simplifications from uncertain candidates. A wrapper can be a required adapter or public API; the audit checks those contracts before recommending removal.

## Apply the skill during planning

Start Claude Code in Plan Mode:

```bash
claude --permission-mode plan
```

Then send this task, replacing the example spec path:

```text
/minimize-code Plan the feature in docs/feature-spec.md.

Read CLAUDE.md, the relevant README sections, the spec,
and the affected code and tests.
Show requirements with source references, code to reuse,
the smallest complete design, affected files and callers,
and proposed verification.

Plan only. Do not edit source or install dependencies.
```

The skill follows the active permission mode. Plan-only requests produce a plan. A Markdown audit file is saved only when the current mode permits that write; otherwise, the report is returned through the permitted chat or plan surface.

The optional [Plan Mode settings](claude-code/optional/plan-default.settings.json) can be merged into `.claude/settings.json` for project terminal sessions. Preserve other settings. Extension-specific instructions are in the full guide.

## Implement or simplify code

Use the same skill for a coding task:

```text
/minimize-code Simplify src/utils.ts while preserving its public API,
invalid-input behavior, and relevant performance requirements.
Reuse existing helpers where they fit and run focused verification.
```

After reviewing an audit, select actual finding IDs for implementation:

```text
/minimize-code Implement only findings A1 and A3 from
CODE_MINIMIZATION_AUDIT.md. Reread CLAUDE.md and the relevant specs.
Preserve required behavior and safety checks. Run focused verification
and update the selected findings with the results.
```

Use your own file paths and finding IDs. A report-only audit leaves implementation changes for a separate request.

## Invoke it in Codex or Cursor

| Tool | Invocation |
| --- | --- |
| Claude Code | `/minimize-code` followed by the task |
| Codex CLI or IDE | Mention `$minimize-code`, or select it with `/skills` |
| Cursor | Type `/`, select `minimize-code`, and supply the task |

For example, in Codex:

```text
Use $minimize-code to audit this repository and create
CODE_MINIMIZATION_AUDIT.md. Read AGENTS.md and the relevant project
specifications first. Change only the report file.
```

For a Cursor audit, select the skill and use the same report-only request with the applicable project instructions.

Explicit invocation selects the workflow for the task. The supplied `CLAUDE.md`, `AGENTS.md`, and always-applied Cursor rule provide guidance from the start of the session, including planning. Description-based automatic selection remains the agent's decision.

## Repository contents

| Path | Purpose |
| --- | --- |
| [START-HERE.md](START-HERE.md) | Short installation and usage instructions |
| [Claude-Code-Setup.md](Claude-Code-Setup.md) | Complete guide, audit and planning workflows, configuration examples, and official references |
| [shared/minimize-code/SKILL.md](shared/minimize-code/SKILL.md) | Canonical portable workflow |
| `shared/minimize-code/agents/openai.yaml` | Optional Codex UI metadata |
| `claude-code/` | Claude Code skill, project instruction snippet, and optional Plan Mode settings |
| `codex/` | Codex skill, UI metadata, and project instruction snippet |
| `cursor/` | Cursor skill and always-applied project rule |

All tool-specific `SKILL.md` copies contain the same workflow. When changing the shared skill, keep those copies in sync. The full guide explains how Cursor and Codex can share a single `.agents/skills/` location and how Claude can import that workflow.

## Validation

The skill metadata has been validated. Sample repository checks covered plan-only behavior and report-only auditing, including preserving a required adapter and public exports while identifying duplicated logic. The report-only check confirmed that only the requested Markdown report was created.

These checks cover the workflow on sample repositories. They do not represent runtime integration tests of every editor version or guarantee a fixed reduction in code, tokens, or cost.
