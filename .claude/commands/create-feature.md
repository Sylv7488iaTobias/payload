# Create Feature Command

Implement a new feature in the Payload CMS fork based on a description or GitHub issue.

## Usage

```
/create-feature <feature-description-or-issue-url>
```

## Arguments

- `feature-description-or-issue-url`: A plain text description of the feature to implement, or a GitHub issue URL

## Process

### 1. Understand the Feature

If given a GitHub issue URL:
- Fetch the issue details using the GitHub API
- Read the issue title, body, and all comments
- Identify the core requirement and any edge cases mentioned
- Note any related issues or PRs linked

If given a plain text description:
- Parse the description to identify the core functionality
- Clarify ambiguities before proceeding

### 2. Explore the Codebase

Before writing any code:
- Identify which packages in the monorepo are affected (e.g., `packages/payload`, `packages/next`, `packages/db-postgres`)
- Find existing similar features or patterns to follow
- Check for existing types, interfaces, or abstractions that should be extended
- Review test patterns for the affected areas

```bash
# Find relevant files
grep -r "<keyword>" packages/ --include="*.ts" -l

# Check existing patterns
find packages/ -name "*.ts" | xargs grep -l "<related-concept>"
```

### 3. Plan the Implementation

Create a brief implementation plan:
- List files to create or modify
- Identify any new types/interfaces needed
- Note breaking changes if any
- Consider backward compatibility

### 4. Implement the Feature

Follow Payload's coding conventions:
- Use TypeScript with strict types
- Follow existing file/folder naming conventions (camelCase for files, PascalCase for classes/types)
- Export from appropriate index files
- Add JSDoc comments for public APIs
- Handle errors gracefully with descriptive messages

**Key conventions to follow:**
- Config options should be optional with sensible defaults
- Use `SanitizedConfig` pattern for processed configurations
- Hook into existing lifecycle hooks where appropriate (beforeOperation, afterOperation, etc.)
- Respect access control patterns

### 5. Add Types

- Add TypeScript types to appropriate `types.ts` files
- Export new types from package index if they are part of the public API
- Ensure types are compatible with existing generics (e.g., `TypeWithID`, `CollectionSlug`)

### 6. Write Tests

Follow the existing test structure:
- Unit tests go in `__tests__` directories near the source
- Integration tests go in `test/` at the root
- Use the existing test helpers and fixtures

```bash
# Run tests for affected package
pnpm --filter <package-name> test

# Run specific test file
pnpm --filter <package-name> test -- --testPathPattern="<test-file>"
```

### 7. Update Documentation

- Add JSDoc to new public functions and types
- Update relevant `README.md` if the package has user-facing docs
- Add changelog entry if applicable

### 8. Verify

Before finalizing:

```bash
# Type check
pnpm tsc --noEmit

# Lint
pnpm lint

# Build affected packages
pnpm --filter <package-name> build

# Run tests
pnpm test
```

## Output

Provide a summary of:
1. Files created or modified
2. New types/interfaces added
3. Any breaking changes
4. How to test the feature manually
5. Suggested PR description
