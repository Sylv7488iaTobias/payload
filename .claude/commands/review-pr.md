# Review Pull Request

Review a pull request for the Payload CMS fork, checking for code quality, consistency with the existing codebase, potential bugs, and adherence to project conventions.

## Usage

```
/review-pr <PR_URL_OR_NUMBER>
```

## Instructions

You are reviewing a pull request for the Payload CMS fork. Follow these steps:

### 1. Gather PR Information

Fetch the PR details:
```bash
gh pr view $ARGUMENTS --json title,body,author,files,additions,deletions,commits,labels,milestone
```

Get the diff:
```bash
gh pr diff $ARGUMENTS
```

List changed files:
```bash
gh pr view $ARGUMENTS --json files --jq '.files[].path'
```

### 2. Understand the Context

- Read the PR title and description carefully
- Identify what problem this PR is solving
- Check if there's a linked issue: look for `Fixes #`, `Closes #`, `Resolves #` in the PR body
- If linked to an issue, fetch issue details:
  ```bash
  gh issue view <ISSUE_NUMBER> --json title,body,labels
  ```

### 3. Code Review Checklist

Evaluate the following areas:

#### TypeScript Quality
- [ ] Proper TypeScript types used (no excessive `any`)
- [ ] Generics used appropriately
- [ ] Interfaces/types are well-defined and reusable
- [ ] No type assertions (`as`) unless absolutely necessary

#### Payload CMS Conventions
- [ ] Follows existing patterns in the codebase
- [ ] Collection/field configurations follow Payload schema conventions
- [ ] Hooks are implemented correctly (beforeChange, afterChange, etc.)
- [ ] Access control patterns are consistent
- [ ] Admin UI components follow existing patterns

#### Code Quality
- [ ] No unnecessary code duplication
- [ ] Functions are focused and single-purpose
- [ ] Variable/function names are descriptive
- [ ] Complex logic has explanatory comments
- [ ] No console.log statements left in production code

#### Testing
- [ ] New features have corresponding tests
- [ ] Bug fixes include regression tests
- [ ] Tests are meaningful and not just checking implementation details
- [ ] Edge cases are covered

#### Breaking Changes
- [ ] Identify any breaking changes to the public API
- [ ] Check if migrations are needed for database schema changes
- [ ] Verify backward compatibility where expected

#### Security
- [ ] No hardcoded secrets or credentials
- [ ] User input is properly validated/sanitized
- [ ] Access control is not weakened
- [ ] No SQL injection or XSS vulnerabilities introduced

#### Performance
- [ ] No obvious N+1 query problems
- [ ] Large data operations are paginated
- [ ] Expensive operations are not run unnecessarily

### 4. Check for Related Files

For each changed file, verify:
- Are there corresponding test files that should also be updated?
- Are there documentation files that need updating?
- Are there translation files that need new keys? (check `.claude/skills/generate-translations/SKILL.md`)
- Are there type definition files that need updating?

### 5. Dependency Audit (if package.json changed)

If `package.json` or `pnpm-lock.yaml` was modified, run the dependency audit skill:
- Review new dependencies for security and license issues
- Check if dev dependencies are correctly categorized
- Verify version pinning strategy is consistent

### 6. Format Your Review

Provide a structured review with:

**Summary**: Brief overview of what the PR does and your overall assessment.

**Verdict**: One of:
- ✅ **APPROVE** - Ready to merge
- 🔄 **REQUEST CHANGES** - Needs work before merging  
- 💬 **COMMENT** - Feedback provided, no blocking issues

**Required Changes** (if any): Numbered list of blocking issues that must be fixed.

**Suggestions** (optional): Non-blocking improvements that would enhance the PR.

**Positive Notes**: Acknowledge good work and patterns worth highlighting.

### 7. Post Review (Optional)

If asked to submit the review:
```bash
# Approve
gh pr review $ARGUMENTS --approve --body "<review_body>"

# Request changes
gh pr review $ARGUMENTS --request-changes --body "<review_body>"

# Comment only
gh pr review $ARGUMENTS --comment --body "<review_body>"
```

## Notes

- Be constructive and specific in feedback
- Reference line numbers when pointing out issues
- Suggest fixes, not just problems
- Consider the PR author's experience level based on their contribution history
- For large PRs (500+ lines), focus on architecture and critical issues first
