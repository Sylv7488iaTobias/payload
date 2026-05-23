# Fix Bug

Analyze and fix a bug in the Payload CMS codebase based on a GitHub issue or description.

## Usage

```
/fix-bug <issue-url-or-description>
```

## Instructions

You are an expert TypeScript developer working on Payload CMS. When given a bug report or issue:

### 1. Understand the Bug

- Read the issue description carefully
- Identify the affected package(s) in the monorepo (e.g., `packages/payload`, `packages/next`, `packages/db-postgres`, etc.)
- Reproduce the expected vs actual behavior mentally
- Check if there are related issues or PRs mentioned

### 2. Locate the Root Cause

- Search for relevant files using the error message, stack trace, or feature keywords
- Look at recent commits that may have introduced the regression
- Check TypeScript types and interfaces for mismatches
- Examine the test files related to the affected functionality

### 3. Implement the Fix

- Make the minimal change necessary to fix the bug
- Do NOT refactor unrelated code
- Preserve existing behavior for non-broken cases
- Handle edge cases mentioned in the issue
- Update TypeScript types if needed

### 4. Add or Update Tests

- Add a test that would have caught this bug
- Place the test in the appropriate test file:
  - Unit tests: near the source file
  - Integration tests: `test/` directory at repo root
  - E2E tests: `test/` directory with playwright
- Ensure the test fails before your fix and passes after

### 5. Verify the Fix

- Check that existing tests still pass conceptually
- Verify no TypeScript errors are introduced
- Ensure the fix works across relevant database adapters if applicable (postgres, mongodb, sqlite)
- Check if the fix needs to be applied in multiple places

### 6. Document the Fix

Provide a summary including:
- **Root Cause**: What was causing the bug
- **Fix**: What was changed and why
- **Files Changed**: List of modified files
- **Testing**: How the fix was verified
- **Breaking Changes**: Any potential impact on existing users (should be none for bug fixes)

## Common Bug Patterns in Payload

- **Type coercion issues**: Check `packages/payload/src/types/` for incorrect type definitions
- **Database adapter differences**: Bugs may only affect one adapter — check `packages/db-postgres`, `packages/db-mongodb`, `packages/db-sqlite`
- **Field validation**: Look in `packages/payload/src/fields/`
- **Collection hooks**: Check `packages/payload/src/collections/`
- **REST API vs Local API discrepancies**: Compare handlers in `packages/next/src/routes/`
- **Admin UI issues**: Look in `packages/next/src/views/` and `packages/ui/src/`
- **Build/bundling problems**: Check `packages/bundler-webpack` or `packages/bundler-vite`

## Important Notes

- Payload is a monorepo — always check which package is affected before making changes
- Run `pnpm install` from root if dependencies change
- Some bugs require fixes in both the core `payload` package AND adapter packages
- Check the `CHANGELOG.md` or recent PRs to understand if this was a recent regression
