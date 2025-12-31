# Repository Guidelines

## Project Structure & Module Organization

- `.claude/agents/` and `.claude/commands/` contain Claude Code agent definitions and command prompts.
- `.codex/prompts/` mirrors prompt content for Codex CLI usage.
- `.github/agents/` and `.github/prompts/` hold GitHub automation agents/prompts used by workflows.
- `.specify/` stores Spec Kit templates and memory files.
- Root files are minimal: `README.md` (overview) and `LICENSE`.

## Build, Test, and Development Commands

- There is no build system or runtime in this repo; changes are mostly Markdown/YAML content.
- Validate changes by reviewing diffs (`git diff`) and ensuring files render correctly in Markdown.
- CI is GitHub Actions only (`.github/workflows/ci.yml`) and runs Claude Code review/bot workflows on PRs.

## Coding Style & Naming Conventions

- Use Markdown for docs and prompts; prefer short headings and fenced code blocks for examples.
- Keep filenames in kebab-case, matching existing patterns like `codex-ask.md` and `speckit.plan.md`.
- YAML files use 2-space indentation (see `.github/workflows/ci.yml`).
- Keep instructions concise and task-focused; avoid long prose in agent/prompt files.

## Testing Guidelines

- No automated test framework is configured.
- If you introduce scripts or tests, add a short “How to run” note in `README.md` and update this guide.

## Commit & Pull Request Guidelines

- Commit messages follow short, imperative summaries (e.g., “Add …”, “Install …”).
- PRs should include a clear description of the change and rationale.
- If you add or rename agents/prompts, update any related listings (for example, `.claude/agents/README.md`) to keep references in sync.
- Avoid tagging automation users unnecessarily; the CI workflow reacts to PR events and `@claude` mentions.

## Agent-Specific Notes

- Agent and prompt files are the product of this repo—treat them as source of truth.
- Prefer small, focused edits per file to keep reviewable diffs and predictable behavior.
