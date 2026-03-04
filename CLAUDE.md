# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code plugin** - a collection of production-ready agents, skills, hooks, commands, rules, and MCP configurations. The project provides battle-tested workflows for software development using Claude Code.

## Commands

```bash
# Full test suite (CI validators + unit tests)
npm test

# Lint JS and Markdown
npm run lint

# Run unit tests only
node tests/run-all.js

# Run a single test file
node tests/lib/utils.test.js

# Run CI validators individually (agents, commands, rules, skills, hooks)
node scripts/ci/validate-agents.js
```

## Architecture

The project is organized into several core components:

- **agents/** - Specialized subagents for delegation (planner, code-reviewer, tdd-guide, jarvis, etc.)
- **skills/** - Workflow definitions and domain knowledge (coding standards, patterns, testing)
- **commands/** - Slash commands invoked by users (/tdd, /plan, /e2e, /jarvis, etc.)
- **hooks/** - Trigger-based automations (session persistence, pre/post-tool hooks); see `hooks/README.md`
- **rules/** - Always-follow guidelines (security, coding style, testing requirements)
- **contexts/** - Pre-built context files for different modes (dev, review, research)
- **mcp-configs/** - MCP server configurations for external integrations
- **schemas/** - JSON schemas for validating agents, hooks, plugins, and package manager config
- **scripts/ci/** - Validators run as part of `npm test` (validate-agents, validate-commands, etc.)
- **scripts/hooks/** - Node.js implementations of hook scripts (cross-platform, no bash required)
- **scripts/lib/** - Shared utilities for hooks and scripts
- **tests/** - Unit and integration test suite

## Key Commands

- `/tdd` - Test-driven development workflow
- `/plan` - Implementation planning
- `/e2e` - Generate and run E2E tests
- `/code-review` - Quality review
- `/build-fix` - Fix build errors
- `/learn` - Extract patterns from sessions
- `/skill-create` - Generate skills from git history

## Development Notes

- Package manager detection: npm, pnpm, yarn, bun (configurable via `CLAUDE_PACKAGE_MANAGER` env var or project config)
- Cross-platform: Windows, macOS, Linux support via Node.js scripts — avoid bash-specific syntax in hooks
- Agent format: Markdown with YAML frontmatter (`name`, `description`, `tools`, `model`)
- Skill format: Each skill is a subdirectory under `skills/` containing a `SKILL.md` with YAML frontmatter
- Hook format: JSON with `matcher` (JMESPath expression) and `hooks` array; exit code `2` blocks PreToolUse, `0` warns, other non-zero logs an error
- CI validation: `scripts/ci/validate-*.js` enforce frontmatter schemas — all agents, commands, rules, and skills must pass before merge
- Schemas in `schemas/` define the expected structure for JSON configs; reference them when adding new hooks or plugin configs

## Contributing

Follow the formats in CONTRIBUTING.md. File naming: lowercase with hyphens (e.g., `python-reviewer.md`, `tdd-workflow.md`).

| Component | Location | Format |
|-----------|----------|--------|
| Agent | `agents/name.md` | Markdown + YAML frontmatter (`name`, `description`, `tools`, `model`) |
| Skill | `skills/name/SKILL.md` | Markdown + YAML frontmatter (`name`, `description`, `origin`) |
| Command | `commands/name.md` | Markdown + `description` frontmatter |
| Hook | `hooks/hooks.json` | JSON with `matcher`, `hooks[]`, `description` |

PR title format: `feat(skills): add rust-patterns skill` / `fix(agents): update tdd-guide`
