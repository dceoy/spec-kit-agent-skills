# cross-agent-skills

Cross‑platform agent skills for AI coding agents

## Overview

This repository provides reusable agent skills and templates for various AI coding assistants, including:

- **Claude Code** - Anthropic's CLI tool
- **GitHub Copilot CLI** - GitHub's terminal assistant
- **Codex CLI** - OpenAI's development tool
- **Spec Kit** - Specification-driven development workflow

## Available Skills

### GitHub Copilot CLI Integration (`.claude/skills/`)

Use GitHub Copilot CLI from within Claude Code:

- **copilot-ask** - Ask questions about code (read-only analysis)
- **copilot-exec** - Execute development tasks (code modifications)
- **copilot-review** - Perform code reviews (quality & security)

**Prerequisites:**

- GitHub Copilot subscription
- Copilot CLI installed: `brew install copilot-cli` or `npm install -g @github/copilot`
- Node.js v22+ and npm v10+

### Codex CLI Integration (`.claude/skills/`)

Use OpenAI Codex CLI from within Claude Code:

- **codex-ask** - Ask questions about code (read-only analysis)
- **codex-exec** - Execute development tasks (code modifications)
- **codex-review** - Perform code reviews (quality & security)

**Prerequisites:**

- ChatGPT Plus/Pro/Team/Enterprise subscription or OpenAI API key
- Codex CLI installed: `npm install -g @openai/codex`

### Spec Kit Workflow (`.claude/skills/`)

Specification-driven development workflow:

- **spec-kit-specify** - Create feature specifications
- **spec-kit-clarify** - Clarify underspecified areas
- **spec-kit-plan** - Generate implementation plans
- **spec-kit-tasks** - Break down into actionable tasks
- **spec-kit-implement** - Execute implementation
- **spec-kit-analyze** - Validate consistency
- **spec-kit-checklist** - Generate quality checklists

## Usage

### From Claude Code

Skills are automatically available in Claude Code. Simply describe your task:

```
User: "Review this code for security issues"
→ Claude may launch copilot-review or codex-review skill

User: "Add email validation to the form"
→ Claude may launch copilot-exec or codex-exec skill

User: "How does authentication work?"
→ Claude may launch copilot-ask or codex-ask skill
```

### From Codex CLI

Codex CLI skills are located in `.codex/skills/`:

- **claude-ask** - Ask Claude questions
- **claude-exec** - Execute tasks with Claude
- **claude-review** - Review code with Claude
- **copilot-ask** - Ask GitHub Copilot CLI questions
- **copilot-exec** - Execute tasks with GitHub Copilot CLI
- **copilot-review** - Review code with GitHub Copilot CLI

## Structure

```
.
├── .claude/
│   ├── agents/          # Claude Code agent definitions
│   ├── commands/        # Claude Code command prompts (Spec Kit)
│   └── skills/          # Claude Code skill definitions
│       ├── copilot-*/   # GitHub Copilot CLI integration
│       ├── codex-*/     # Codex CLI integration
│       └── spec-kit-*/  # Spec Kit workflow
├── .codex/
│   ├── prompts/         # Codex CLI prompts
│   └── skills/          # Codex CLI skill definitions
├── .github/
│   ├── agents/          # GitHub automation agents
│   ├── prompts/         # GitHub workflow prompts
│   └── workflows/       # CI/CD workflows
└── .specify/            # Spec Kit templates

```

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/your-org/cross-agent-skills.git
   ```

2. **Install desired CLI tools:**

   For GitHub Copilot CLI:

   ```bash
   brew install copilot-cli
   # or
   npm install -g @github/copilot
   ```

   For Codex CLI:

   ```bash
   npm install -g @openai/codex
   ```

3. **Authenticate:**

   GitHub Copilot CLI:

   ```bash
   copilot
   # Then run: /login
   ```

   Codex CLI:

   ```bash
   codex
   # Follow authentication prompts
   ```

## Contributing

Contributions are welcome! When adding new skills:

1. Follow existing naming conventions (kebab-case)
2. Use the SKILL.md format with YAML frontmatter
3. Document prerequisites, usage, and examples
4. Update this README with new skills
5. Follow commit message conventions (imperative, concise)

See [AGENTS.md](AGENTS.md) for detailed guidelines.

## License

See [LICENSE](LICENSE) for details.
