# Clean Code

Be concise, direct, and solution-focused. Write working code, not tutorials.

## Core Principles

| Principle | Rule |
|-----------|------|
| SRP | Single Responsibility — each function/class does ONE thing |
| DRY | Extract duplicates, reuse |
| KISS | Simplest solution that works |
| YAGNI | Don't build unused features |
| Boy Scout | Leave code cleaner than you found it |

Start simple. Add complexity only when proven necessary — removing it later is much harder than adding it.

## Naming

| Element | Convention |
|---------|------------|
| Variables | Reveal intent: `userCount` not `n` |
| Functions | Verb + noun: `getUserById()` |
| Booleans | Question form: `isActive`, `hasPermission`, `canEdit` |
| Constants | SCREAMING_SNAKE: `MAX_RETRY_COUNT` |

If you need a comment to explain a name, rename it.

## Functions

- Small: aim 5-10 lines, max ~20.
- One thing, one level of abstraction.
- Max 3 arguments, prefer 0-2.
- No unexpected mutation of inputs.

## Structure

- Guard clauses / early returns for edge cases.
- Flat over nested (max ~2 levels).
- Compose small functions. Keep related code close.

## Anti-patterns

| Don't | Do |
|-------|-----|
| Comment every line | Delete obvious comments |
| Helper for a one-liner | Inline it |
| Factory for 2 objects | Direct instantiation |
| `utils.ts` with 1 function | Put code where it's used |
| Deep nesting | Guard clauses |
| Magic numbers | Named constants |
| God functions | Split by responsibility |

## Before editing a file

- What imports this file? They might break.
- What does this file import? Interface changes ripple.
- What tests cover it? Is it a shared component?
- Edit the file and all dependent call sites in the same change. Never leave broken imports.
