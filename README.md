# dotclaude

Personal configuration for [Claude Code](https://github.com/anthropics/claude-code).

## Contents

- `CLAUDE.md` - Engineering guidelines (development principles, testing standards, commit messages)
- `agents/` - Custom agent definitions (code-reviewer, subject-matter-expert)
- `settings.json.example` - Example settings template

## Setup

Clone to your home directory:

```bash
git clone https://github.com/nchapman/dotclaude.git ~/.claude
cp ~/.claude/settings.json.example ~/.claude/settings.json
```

Or symlink if you keep dotfiles elsewhere:

```bash
ln -s /path/to/dotclaude ~/.claude
```

## Usage

The `CLAUDE.md` file is automatically loaded by Claude Code and applies to all projects. Project-specific instructions can override these in each project's own `CLAUDE.md`.

Custom agents are available via the Task tool when working in Claude Code.
