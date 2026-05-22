# TypeScript Conventions

## Types
- Prefer `interface` over `type` for object shapes that may be extended
- Use `type` for unions, intersections, and utility types
- Define shared types in `src/types/` — colocate types with their module when they're only used there

## Naming
- PascalCase for types, interfaces, enums, and class names
- camelCase for variables, functions, and class instances
- Prefix event handlers with `handle` (e.g., `handleClick`)

## Imports
- Use `import type` for type-only imports
- Group imports: external → internal → types
- No default exports — prefer named exports

## Strictness
- `strict: true` in tsconfig — no exceptions
- Avoid `any` — use `unknown` when the type is not known
- Use `as const` for literal types and tuples
- Prefer `const` over `let` — use `let` only when reassignment is necessary

## Patterns
- Use discriminated unions for state machines and API responses
- Use branded types for domain primitives (e.g., `UserId` instead of `string`)
- Use `satisfies` operator to validate types without widening

For testing TypeScript code, see TESTING.md.
