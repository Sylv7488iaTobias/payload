# Update Translations

This command helps update or add translation strings across all locale files in the Payload CMS project.

## Usage

```
/update-translations [locale?] [key?] [value?]
```

## Arguments

- `locale` (optional): Specific locale to update (e.g., `de`, `fr`, `es`). If omitted, updates all locales.
- `key` (optional): The translation key path (e.g., `general.save`, `fields.required`)
- `value` (optional): The translated value to set

## What This Command Does

1. **Scans** the `packages/translations/src` directory for all locale files
2. **Identifies** missing or outdated translation keys compared to the `en` base locale
3. **Generates** missing translations using context from surrounding keys and existing translations
4. **Validates** that all locale files have consistent key structures
5. **Reports** a summary of changes made

## Steps

### 1. Discover Translation Files

First, find all translation files:

```bash
find packages/translations/src -name '*.ts' | sort
```

The base locale is always `en.ts`. All other locales should mirror its structure.

### 2. Parse the Base Locale

Read `packages/translations/src/languages/en.ts` to understand the full key structure.

Pay attention to:
- Nested object structure
- Interpolation variables like `{{count}}`, `{{label}}`, `{{field}}`
- Pluralization patterns
- Context comments above translation groups

### 3. Check Target Locale(s)

For each locale file:
- Compare keys against `en.ts`
- Identify:
  - **Missing keys**: Present in `en` but not in target locale
  - **Extra keys**: Present in target but not in `en` (may be outdated)
  - **Type mismatches**: Key exists but value type differs

### 4. Apply the Generate Translations Skill

Use the skill at `.claude/skills/generate-translations/SKILL.md` to:
- Generate contextually appropriate translations for missing keys
- Preserve existing translations that are already correct
- Maintain the TypeScript type safety of the locale files

### 5. Validate Output

After updating, verify:

```bash
# Type-check the translations package
cd packages/translations && pnpm tsc --noEmit

# Run any translation-specific tests
pnpm test --filter=@payloadcms/translations
```

### 6. Format Files

Ensure all modified files are properly formatted:

```bash
pnpm prettier --write packages/translations/src/**/*.ts
```

## Output Format

After completion, provide a summary:

```
## Translation Update Summary

### Files Modified
- packages/translations/src/languages/de.ts (+12 keys)
- packages/translations/src/languages/fr.ts (+8 keys)

### Keys Added
- `general.unsavedChanges` → de, fr, es, pt
- `fields.blocks.addLabel` → de, fr

### Warnings
- `es.ts`: Key `deprecated.oldKey` exists but not in base locale
- `ja.ts`: Could not auto-translate 3 keys (manual review needed)

### Next Steps
1. Review auto-generated translations with native speakers
2. Open PR with changes
3. Tag relevant locale maintainers for review
```

## Notes

- Never modify `en.ts` unless explicitly asked — it is the source of truth
- Preserve TypeScript `as const` assertions and type exports
- When a translation cannot be confidently generated, leave the English fallback and add a `// TODO: translate` comment
- Check `CONTRIBUTING.md` in the translations package for locale-specific guidelines
