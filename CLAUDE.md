# Engineering Guidelines

## Development Principles
- **Readability**: Use clear names and logical structure. Comment intent, not action.
- **Modularity**: Small, single-purpose functions. Avoid deep nesting, many branches, or interleaved concerns.
- **Pragmatism**: Solve the immediate problem. Avoid speculative abstractions.
- **Explicitness**: Validate at boundaries. Fail fast with useful error messages.
- **Consistency**: Follow existing codebase patterns and naming conventions.
- **Cleanliness**: Delete dead code (verify with grep—it may be called dynamically). Only refactor what you understand and can test.
- **Testability**: Favor pure functions, clear I/O, and limited side effects.

## Testing Standards
- **Behavior-Focused**: Verify *what* code does, not *how*. Refactoring shouldn't break tests.
- **Atomic**: One concept per test. Follow the "Arrange, Act, Assert" pattern.
- **Documentation**: Use tests to explain system behavior through clear naming.
- **Realistic Mocks**: Mock external boundaries only. Don't mock internal collaborators.
- **Comprehensive**: Always test edge cases, empty inputs, and error paths.
- **Reliable**: Tests must be fast, deterministic, and isolated.
- **Regression-Proof**: Bug found? Write a failing test first, then fix it.

## Commit Messages
- **Match the repo's style**: Follow existing subject line conventions (check `git log --oneline`).
- **Imperative mood**: "Add feature" not "Added feature" or "Adds feature".
- **Bullet the changes**: After the subject line, include a concise list of meaningful changes.
- **Explain what and why**: The diff shows *how*—the message should explain *what* changed and *why*.
- **Keep it scannable**: Future readers (and agents) use git history to understand recent changes.

## Bulk Find-and-Replace Protocol
Use for uniform patterns across 20+ files. Otherwise, use standard editing tools.

1. **Scope**: Define file list explicitly (e.g., `FILES=$(grep -rl 'old' --include='*.ts' src/)`).
2. **Dry Run**: Preview changes using `diff` before applying (e.g., `for f in $FILES; do perl -pe 's/\Qold\E/new/g' "$f" | diff -u "$f" -; done`).
3. **Execute**: Use Perl with backup: `perl -i.bak -pe 's/\Qold\E/new/g' $FILES`.
4. **Verify**: Ensure no matches remain and functionality is intact.
5. **Cleanup**: Remove backups: `find . -name "*.bak" -delete`.

**Safety Rules**:
- Use `\Q...\E` for literal strings and `\b` for word boundaries.
- Always use `-i.bak` for rollback capability.
- Never run on `node_modules/`, `.git/`, `dist/`, or minified files.
- Pause if changes exceed 100+ matches or modify function signatures.
