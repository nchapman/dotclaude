## Guidelines

1. **Prioritize readability.** Write code that's easy to follow. Use clear names, logical structure, and add brief comments where intent isn't obvious.
2. **Keep functions small and focused.** Each function should do one thing. If a function exceeds 20-30 lines, look for opportunities to extract smaller functions with clear names.
3. **Name things precisely.** Variable and function names should describe what they hold or do. Avoid abbreviations unless they're universal. Don't be afraid of longer names if they're clearer.
4. **Don't over-engineer.** Solve the problem at hand. Avoid unnecessary abstractions, speculative features, or patterns that aren't pulling their weight.
5. **Handle errors explicitly.** Don't let failures pass silently. Validate inputs at boundaries, catch exceptions deliberately, and provide useful error messages.
6. **Follow existing conventions.** Match the style, patterns, and structure already present in the codebase. Consistency matters more than personal preference.
7. **Favor simple, elegant solutions.** Prefer straightforward approaches. The best code often feels obvious in hindsight — it fits the problem without forcing. If you're fighting the code, step back and look for a better shape.
8. **Delete dead code.** Remove unused functions, commented-out blocks, and obsolete logic. Don't leave clutter.
9. **Make it testable.** Write code that's easy to test — pure functions where possible, limited side effects, clear inputs and outputs.
10. **Refactor as you go.** If you touch messy code, leave it a little better than you found it, but be careful not to break it.

## Best Practices

## Bulk Find-and-Replace

**Tool Selection:**
- <20 files or context-dependent: Use `Edit` tool
- >20 files with uniform pattern: Use `perl -i.bak`

**CLI Protocol:**
```bash
# 1. Build explicit file list (never unbounded globs)
FILES=$(grep -rl 'pattern' --include='*.ts' src/)

# 2. Dry run - preview changes (never skip)
for f in $FILES; do
  perl -pe 's/\Qold\E/new/g' "$f" | diff -u "$f" -
done

# 3. Execute with backup
perl -i.bak -pe 's/\Qold\E/new/g' $FILES

# 4. Verify no matches remain
grep -l 'old' $FILES && echo "ERROR" || echo "OK"

Critical Rules:
- Always use perl over sed (cross-platform consistent)
- Always use -i.bak (never -i alone—no rollback without backup)
- Use \Q...\E for literal strings (auto-escapes special chars)
- Use \b for word boundaries (\boldName\b won't match boldNameExtra)
- Always preview before executing

Auto-Skip:
- node_modules/, vendor/, .git/, dist/, build/
- Binary files, *.min.js, *.min.css, *.map

Escalate If:
- 100+ matches, multiple languages, or changes function signatures
