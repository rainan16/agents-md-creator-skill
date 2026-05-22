---
name: agents-md-creator
description: >
  Create and refactor AGENTS.md files following progressive disclosure principles.
  Use this skill whenever the user mentions AGENTS.md, CLAUDE.md, agent configuration, AI coding agent setup, agent instructions, or wants to set up rules for how AI agents should behave in their repository.
  Also use when the user wants to reduce the size of their AGENTS.md, split it into smaller files, organize agent instructions, apply progressive disclosure, or fix a bloated AGENTS.md.
  This skill handles both creating a minimal AGENTS.md from scratch and refactoring an existing one that has grown too large.
  Pay attention to instructions about instruction budget, one-liner project descriptions, and moving domain-specific rules to separate reference files.
license: Apache-2.0
metadata:
  author: rainan16
  version: "1.1.0"
---

# agents-md-creator

A skill for creating and refactoring `AGENTS.md` files using progressive disclosure principles, based on the methodology from [A Complete Guide To AGENTS.md](https://www.aihero.dev/a-complete-guide-to-agents-md).

## Core Principles

These principles guide every decision when creating or refactoring an AGENTS.md:

- **Instruction budget**: Frontier models can follow ~150-200 instructions. Every token in AGENTS.md loads on every request. Keep AGENTS.md as small as possible so more tokens are available for task-specific work.
- **One-liner project description**: A single sentence that anchors every decision the agent makes (e.g., "This is a React component library for accessible data visualization.").
- **Package manager**: Specify if not npm. Otherwise the agent may default to incorrect commands.
- **Build/typecheck commands**: Only include if non-standard.
- **Progressive disclosure**: Move domain-specific rules (TypeScript conventions, testing patterns, API design, Git workflow) to separate files referenced from the root AGENTS.md. Those rules only load when the agent needs that domain.
- **Document capabilities, not file paths**: File paths change. Describe what the project does and where things might be. Domain concepts (e.g., "organization" vs "group" vs "workspace") are more stable than paths — prefer those.
- **Stale docs poison context**: Keep everything minimal and avoid documenting file system structure.

## Creating a New AGENTS.md

When the user asks you to create/set up an AGENTS.md for their project:

### Step 1: Analyze the project

First, explore the project to understand:
- What does the project do? (read package.json, README, configs, etc.)
- What package manager is used? (check for pnpm-lock.yaml, yarn.lock, bun.lock, package-lock.json)
- What are the build, test, and typecheck commands? (check package.json scripts)
- What tech stack and frameworks are used? (React, Node, Python, Go, etc.)
- Is this a monorepo? (check for workspaces config, multiple packages/)

### Step 2: Write the root AGENTS.md

Create a single root `AGENTS.md` with only:

```markdown
# Project

[One-sentence project description]

## Commands

- Build: [command]
- Test: [command]
- Typecheck: [command]
- Package manager: [name]

## Reference

For domain-specific guidance, see the files under docs/.
```

Adjust the format to match the project's needs, but keep to these essentials. Do NOT add anything that isn't relevant to every single task in the repo.

### Step 3: Infer categories and create reference files

Scan the project to determine what reference file categories make sense:

| If the project uses...              | Create reference file...     |
| ----------------------------------- | ---------------------------- |
| TypeScript                          | `docs/TYPESCRIPT.md`         |
| A test framework (vitest, jest, pytest, etc.) | `docs/TESTING.md` |
| A build tool (esbuild, webpack, vite, etc.) | `docs/BUILD.md`    |
| Git                                 | `docs/GIT.md`                |
| Docker                              | `docs/DOCKER.md`             |
| A specific framework (Next.js, Django, etc.) | `docs/FRAMEWORK_GUIDE.md` |
| API design patterns                 | `docs/API_CONVENTIONS.md`    |
| Database/Prisma/ORM                 | `docs/DATABASE.md`           |
| CI/CD pipelines                     | `docs/CI_CD.md`              |
| CSS/styling conventions             | `docs/STYLING.md`            |
| Architecture decisions              | `docs/ARCHITECTURE.md`       |

For each category:
1. Scan existing code to extract actual conventions being followed
2. Look at existing config files, lint rules, and patterns in the codebase
3. Write the reference file with clear, actionable guidance specific to this project

Each reference file should be self-contained and follow the same progressive disclosure pattern — it can reference other docs/ files if needed:

```
docs/
├── TYPESCRIPT.md
│   └── references TESTING.md
├── TESTING.md
│   └── references specific test runners
├── BUILD.md
├── ARCHITECTURE.md
└── ...
```

### Step 4: Validate

After creating the AGENTS.md and reference files:
- Read through the root AGENTS.md — is every line essential to every task?
- Check that no file paths (like `src/auth/handlers.ts`) are hardcoded in AGENTS.md
- Verify reference files contain actionable, specific guidance (not vague platitudes)

## Refactoring an Existing AGENTS.md

When the user asks you to refactor/fix/clean up their AGENTS.md:

### Step 1: Analyze the existing file

Read the current AGENTS.md thoroughly. Identify:
- **Contradictions**: Instructions that conflict with each other
- **Essentials**: What belongs in the root (project description, package manager, build commands)
- **Groupable instructions**: Rules that can be moved to domain-specific reference files
- **Redundancies**: Things the agent already knows (e.g., "write clean code", "use const instead of let")
- **Stale file paths**: Any hardcoded file paths that may be outdated

### Step 2: Flag contradictions

For each contradiction found, ask the user which version to keep. Do NOT guess.

### Step 3: Group and create reference files

Organize remaining instructions into logical categories. For each category:
1. Create a markdown file under `docs/`
2. Move the relevant instructions there
3. Refine the instructions to be specific and actionable (remove vague or obvious rules)

### Step 4: Write the minimal root AGENTS.md

Create the replacement root AGENTS.md with only:
- One-sentence project description
- Package manager
- Non-standard build/typecheck commands
- Links to reference files under docs/

### Step 5: Flag for deletion

Present the user with a list of instructions you recommend deleting:
- Redundant (agent already knows it)
- Too vague to be actionable (e.g., "write clean code")
- Overly obvious

### Step 6: Validate

Same validation as Step 4 of creation.

## Monorepo Support

For monorepos, use multi-level AGENTS.md:

- **Root**: Monorepo purpose, how to navigate packages, shared tooling, workspace config
- **Package**: Package purpose, specific tech stack, package-specific conventions

The agent sees all merged AGENTS.md files — keep each level focused on what's relevant at that scope.

### Monorepo example

Root `AGENTS.md`:
```markdown
# Project
This is a monorepo containing web services and CLI tools.

## Commands
- Package manager: pnpm
- Install: pnpm install
- Build all: pnpm -r build

## Reference
See each package's AGENTS.md for specific guidelines.
```

Package `packages/api/AGENTS.md`:
```markdown
# Package: API
This package is a Node.js GraphQL API using Prisma.

## Reference
See docs/API_CONVENTIONS.md for API design patterns.
```

## Progressive Disclosure File Structure

The overall structure should look like this:

```
AGENTS.md              # Minimal root file
docs/
├── TYPESCRIPT.md      # TypeScript conventions
├── TESTING.md         # Testing patterns
├── BUILD.md           # Build tool configuration
├── GIT.md             # Git workflow
├── DOCKER.md          # Docker conventions
├── API_CONVENTIONS.md # API design
└── ...                # Project-specific categories
```

Reference files can nest deeper:
```markdown
<!-- docs/TYPESCRIPT.md -->
# TypeScript Conventions

For testing TypeScript code, see TESTING.md.
```

## What NOT to do

- Do NOT generate AGENTS.md from an initialization script template — those prioritize comprehensiveness over restraint
- Do NOT include "always" or "never" in all-caps — use conversational, explanatory language
- Do NOT add rules about things the agent already knows (e.g., "use const instead of let")
- Do NOT hardcode file paths in AGENTS.md — describe capabilities, not locations
- Do NOT create reference files for categories that don't apply to the project
- Do NOT use this skill for README files, CONTRIBUTING.md, documentation, or human-oriented guides — this is specifically for AI agent configuration

## Example output

A well-crafted AGENTS.md:

```markdown
# Project
This is a Next.js e-commerce application with a PostgreSQL database.

## Commands
- Package manager: pnpm
- Dev: pnpm dev
- Build: pnpm build
- Test: pnpm test
- Typecheck: pnpm typecheck

## Reference
- TypeScript conventions: docs/TYPESCRIPT.md
- Testing patterns: docs/TESTING.md
- Styling: docs/STYLING.md
- API conventions: docs/API_CONVENTIONS.md
```
