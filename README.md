# agents-md-creator

A skill for AI coding agents that creates and refactors `AGENTS.md` files using progressive disclosure principles.

## What it does

**Create** a minimal `AGENTS.md` from scratch for any project — a one-liner description, package manager, essential commands, and links to domain-specific reference files.

**Refactor/Optimize** an existing bloated `AGENTS.md` by splitting it into a minimal root file and separate reference files organized by domain (TypeScript, testing, build, Git, Docker, etc.). Vague or redundant instructions are flagged for removal.

Both operations follow progressive disclosure: the root file stays small so every token loaded on every AI request is essential. Domain-specific rules live in separate files under `docs/` and are only loaded when the agent needs them.

## When to use

- Setting up a new project and need an `AGENTS.md` for AI agents
- Your existing `AGENTS.md` has grown too large and needs cleanup
- Setting up a monorepo with package-level `AGENTS.md` files
- Applying progressive disclosure patterns to agent configuration

## How it works

The skill scans your project to understand its tech stack, package manager, and conventions, then:

1. Writes a one-sentence project description that anchors the agent's understanding
2. Specifies the package manager and non-standard build commands
3. Infers relevant categories from your codebase (TypeScript, testing, Docker, etc.)
4. Creates `docs/` reference files for each category with specific, actionable guidance
5. Keeps the root `AGENTS.md` minimal — no hardcoded file paths, no obvious rules

For monorepos, it creates multi-level `AGENTS.md` files: a root file for shared tooling and workspace navigation, with package-level files for domain-specific guidance.

## Agent Skills Specification

The skill conforms to the [agentskills.io specification](https://agentskills.io/specification).
