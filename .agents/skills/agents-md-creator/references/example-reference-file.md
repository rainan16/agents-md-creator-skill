# TypeScript Conventions

## Types
- Prefer `interface` over `type` for object shapes that may be extended
- Use `type` for unions, intersections, and utility types
- Colocate types with their module; shared types live in a dedicated types directory

## Naming
- Prefix event handlers with `handle` (e.g., `handleClick`, `handleSubmit`)
- No default exports — prefer named exports

## Imports
- Use `import type` for type-only imports
- Group imports: external → internal → types

## Strictness
- `strict: true` in tsconfig — no exceptions
- Avoid `any` — use `unknown` when the type is not known
- Use `as const` for literal types and tuples

## Patterns
- Use discriminated unions for state machines and API responses
- Use branded types for domain primitives (e.g., `UserId` instead of `string`)
- Use `satisfies` operator to validate types without widening

For testing TypeScript code, see TESTING.md.
