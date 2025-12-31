# Repository Guidelines

## Project Structure & Module Organization

- `.claude/agents/` contains Claude Code agent definitions.
- `.claude/commands/` contains Claude Code command prompts (Spec Kit commands).
- `.claude/skills/` contains Claude Code skill definitions (copilot-_, codex-_, spec-kit-\*).
- `.codex/prompts/` mirrors Spec Kit prompt content for Codex CLI usage.
- `.codex/skills/` contains Codex CLI skill definitions (claude-_ and copilot-_).
- `.github/agents/` and `.github/prompts/` hold GitHub automation agents/prompts used by workflows.
- `.github/workflows/ci.yml` runs Claude Code review/bot workflows on PRs.
- `.specify/` stores Spec Kit templates and memory files.
- Root files are minimal: `README.md` (overview), `LICENSE`, `AGENTS.md`, and `CLAUDE.md`.

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

## Serena MCP Usage (Prioritize When Available)

- **If Serena MCP is available, use it first.** Treat Serena MCP tools as the primary interface over local commands or ad-hoc scripts.
- **Glance at the Serena MCP docs/help before calling a tool** to confirm tool names, required args, and limits.
- **Use the MCP-exposed tools for supported actions** (e.g., reading/writing files, running tasks, fetching data) instead of re-implementing workflows.
- **Never hardcode secrets.** Reference environment variables or the MCP's configured credential store; avoid printing tokens or sensitive paths.
- **If Serena MCP isn't enabled or lacks a needed capability, say so and propose a safe fallback.** Mention enabling it via `.mcp.json` when relevant.
- **Be explicit and reproducible.** Name the exact MCP tool and arguments you intend to use in your steps.
