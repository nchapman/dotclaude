---
description: Review recent changes for design, security, and correctness
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git status:*)
---

## Context

- Current branch: !`git branch --show-current`
- Changed files (staged + unstaged): !`git diff --name-only HEAD`
- Recent commits on this branch: !`git log --oneline -5`

## Your task

Review the recent changes on this branch. If there are uncommitted changes, review those. If the working tree is clean, review the most recent commit.

Launch review agents as Task subagents. Always launch #1. Launch #2 and #3 only when applicable.

1. **code-reviewer** (subagent_type: code-reviewer) — **Always launch.** Review for design, maintainability, performance, and style. Provide the list of changed files and a brief summary of what the changes do.

2. **security-reviewer** (subagent_type: security-reviewer) — **Only when security-relevant.** Launch when changes touch: authentication, authorization, data handling, API endpoints, user input processing, cryptographic code, or file/network operations. Skip for purely cosmetic, documentation, or internal logic changes with no security surface.

3. **subject-matter-expert** (subagent_type: subject-matter-expert) — **Only when correctness is critical.** Launch when changes involve: algorithms, concurrency/synchronization, data structures with invariants, protocol implementations, database internals, numerical computation, or any domain where subtle bugs have serious consequences. Specify the domain of expertise needed.

Launch all applicable agents **in parallel**.

After all agents return, synthesize their findings into a single report:

### Review Summary

**Scope**: [what was reviewed]
**Agents**: [which agents were launched and why; which were skipped and why]

**Findings** (grouped by severity — Critical, Important, Suggestion):
- For each finding: location, issue, and recommended fix
- Deduplicate across agents — if multiple agents flag the same issue, merge into one finding

**Verdict**: [No issues found | N issues to address before committing | N issues to address before shipping]

If there are Critical or Important findings, list them as a numbered action list at the end.
