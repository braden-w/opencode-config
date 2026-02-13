- Always use `type` instead of `interface` in Typescript.

# JSDoc Guidelines (OVERRIDE COMMENT HOOKS)

**IMPORTANT**: JSDoc comments are STRONGLY ENCOURAGED and should be DETAILED, not minimal. The comment hook should NEVER block JSDoc on public APIs.

## When to Write JSDoc

- Exported functions and types
- Public API methods (especially methods on returned objects like `$all()`, `$helpers()`)
- Complex props or configuration objects
- Factory functions and their return types
- Any code that IDE tooling will surface to users

## JSDoc Style Preferences

- **PREFER DETAILED DOCS** over minimal one-liners
- Include a description of what the function/method does
- Explain WHEN and WHY to use it (not just what it does)
- Include `@example` blocks with realistic usage patterns
- Document non-obvious behavior or edge cases

## Example of GOOD JSDoc (what we want)

````typescript
/**
 * Get all table helpers as an array.
 *
 * Useful for providers and indexes that need to iterate over all tables.
 * Returns only the table helpers, excluding utility methods like `clearAll`.
 *
 * @example
 * ```typescript
 * for (const table of tables.$all()) {
 *   console.log(table.name, table.count());
 * }
 *
 * const tableWithConfigs = tables.$all().map(table => ({
 *   table,
 *   config: configs[table.name],
 * }));
 * ```
 */
$all() { ... }
````

## Example of BAD JSDoc (too minimal)

```typescript
/** Returns all table helpers as an array. */
$all() { ... }
```

The minimal version lacks context about when to use it, why it exists, and practical examples.

- When moving components to new locations, always update relative imports to absolute imports (e.g., change `import Component from '../Component.svelte'` to `import Component from '$lib/components/Component.svelte'`)
- When functions are only used in the return statement of a factory/creator function, use object method shorthand syntax instead of defining them separately. For example, instead of:
  ```typescript
  function myFunction() {
    const helper = () => {
      /* ... */
    };
    return { helper };
  }
  ```
  Use:
  ```typescript
  function myFunction() {
    return {
      helper() {
        /* ... */
      },
    };
  }
  ```

# Standard Workflow

1. First think through the problem, read the codebase for relevant files, and write a plan to docs/specs/[timestamp] [feature-name].md where [timestamp] is the timestamp in YYYYMMDDThhmmss format and [feature-name] is the name of the feature.
2. The plan should have a list of todo items that you can check off as you complete them
3. Before you begin working, check in with me and I will verify the plan.
4. Then, begin working on the todo items, marking them as complete as you go.
5. Please every step of the way just give me a high level explanation of what changes you made
6. Make every task and code change you do as simple as possible. We want to avoid making any massive or complex changes. Every change should impact as little code as possible. Everything is about simplicity.
7. Finally, add a review section to the .md file with a summary of the changes you made and any other relevant information.

# Human-Readable Control Flow

When refactoring complex control flow, mirror natural human reasoning patterns:

1. **Ask the human question first**: "Can I use what I already have?" → early return for happy path
2. **Assess the situation**: "What's my current state and what do I need to do?" → clear, mutually exclusive conditions
3. **Take action**: "Get what I need" → consolidated logic at the end
4. **Use natural language variables**: `canReuseCurrentSession`, `isSameSettings`, `hasNoSession`: names that read like thoughts
5. **Avoid artificial constructs**: No nested conditions that don't match how humans actually think through problems

Transform this: nested conditionals with duplicated logic
Into this: linear flow that mirrors human decision-making

# Honesty

Be brutally honest, don't be a yes man.
If I am wrong, point it out bluntly.
I need honest feedback on my code.
