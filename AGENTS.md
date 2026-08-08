# Repository Guidelines

## Project Structure & Module Organization

- `skills/` is the canonical source for shared skills.
- `.claude/skills` and `.agents/skills` expose the shared skills to Claude Code and Codex CLI.
- Legacy Spec Kit layouts such as `.claude/commands/`, `.codex/prompts/`, and `.opencode/command/` must not be reintroduced unless a supported integration explicitly requires them.

## Build, Test, and Development Commands

- `bash .specify/scripts/bash/check-prerequisites.sh --help` lists supported validation modes.
- `bash .specify/scripts/bash/check-prerequisites.sh --paths-only` prints the active feature paths without modifying files.

## Coding Style & Naming Conventions

- Use concise Markdown with ATX headings and direct, imperative instructions.
- Keep skill directories in kebab-case and limit each directory to one `SKILL.md`.
- Preserve stable front matter keys such as `name` and `description`.
- For Bash, follow the existing style in `.specify/scripts/bash/*.sh`: `#!/usr/bin/env bash`, quoted variables, small functions, and defensive checks.
- Keep intentionally maintained runtime files synchronized with the current Spec Kit output and shared skills.

## Testing Guidelines

- No dedicated unit-test framework or coverage gate is checked into this repo.
- Validate changes by smoke-testing current Bash helpers with `--help`, `--json`, or `--paths-only` where supported.
- Verify Markdown renders cleanly.
- Confirm current runtime-specific files stay synchronized with the shared skills and that obsolete runtime layouts are absent.

## Commit & Pull Request Guidelines

- Format Markdown files with `npx prettier --write` before committing.
- Lint Bash scripts with `shellcheck`, YAML files with `yamllint`, and GitHub Actions workflow files with `actionslint`, `zizmor --fix=safe`, and `checkov --framework=all`.
- Keep PRs focused and include: concise summary, affected workflow paths, and linked issue/context.
- Branch names use appropriate prefixes on creation (e.g., `feature/...`, `bugfix/...`, `refactor/...`, `docs/...`, `chore/...`).
- When instructed to create a PR, create it as a draft with appropriate labels by default.

## Spec Kit Workflow

- Keep workflow content synchronized across skills and the runtime files that are still maintained by the current Spec Kit integrations.
- The canonical order in this repo is `constitution -> specify` or `baseline -> clarify -> plan -> tasks -> analyze -> implement`.
- If you add or rename a step, update the source skill and every affected maintained runtime entry point in the same change.
