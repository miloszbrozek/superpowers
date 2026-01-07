---
name: using-git-worktrees
description: Use when starting feature work that needs isolation from current workspace or before executing implementation plans - creates isolated git worktrees with smart directory selection and safety verification
---

# Using Git Worktrees

## Overview

Git worktrees create isolated workspaces sharing the same repository, allowing work on multiple branches simultaneously without switching.

**Core principle:** Systematic directory selection + safety verification = reliable isolation.

**Announce at start:** "I'm using the using-git-worktrees skill to set up an isolated workspace."

## Directory Selection Process

Follow this priority order:

### 1. Check Existing Directory

```bash
# Check for worktrees directory
ls -d worktrees 2>/dev/null
```

**If found:** Use `worktrees/` directory.

### 2. Check CLAUDE.md

```bash
grep -i "worktree.*director" CLAUDE.md 2>/dev/null
```

**If preference specified:** Use it without asking.

### 3. Default to worktrees/

If no directory exists and no CLAUDE.md preference, use `worktrees/` (project-local).

For global location preference, user can specify in CLAUDE.md:
```
Worktree directory: ~/.config/superpowers/worktrees/
```

## Safety Verification

### For Project-Local Directory (worktrees/)

**MUST verify directory is ignored before creating worktree:**

```bash
# Check if directory is ignored (respects local, global, and system gitignore)
git check-ignore -q worktrees 2>/dev/null
```

**If NOT ignored:**

Per Jesse's rule "Fix broken things immediately":
1. Add appropriate line to .gitignore
2. Commit the change
3. Proceed with worktree creation

**Why critical:** Prevents accidentally committing worktree contents to repository.

### For Global Directory (~/.config/superpowers/worktrees)

No .gitignore verification needed - outside project entirely.

## Creation Steps

### 1. Detect Project Name

```bash
project=$(basename "$(git rev-parse --show-toplevel)")
```

### 2. Create Worktree

```bash
# Determine full path
case $LOCATION in
  worktrees)
    path="worktrees/$BRANCH_NAME"
    ;;
  ~/.config/superpowers/worktrees/*)
    path="~/.config/superpowers/worktrees/$project/$BRANCH_NAME"
    ;;
esac

# Create worktree with new branch
git worktree add "$path" -b "$BRANCH_NAME"
cd "$path"
```

### 3. Symlink Shared Files

Symlink files from main worktree that are gitignored but needed for runtime:

```bash
# Get path to main worktree
main_worktree=$(git worktree list | head -1 | awk '{print $1}')

# Symlink .env if exists in main
if [ -f "$main_worktree/.env" ] && [ ! -e ".env" ]; then
  ln -s "$main_worktree/.env" .env
  echo "Symlinked .env"
fi

# Symlink .venv if exists in main (saves disk space and install time)
if [ -d "$main_worktree/.venv" ] && [ ! -e ".venv" ]; then
  ln -s "$main_worktree/.venv" .venv
  echo "Symlinked .venv"
fi
```

**Why symlink these files:**
- `.venv` - Saves 5-10GB disk space and minutes of install time per worktree
- `.env` - API keys and credentials needed to run app/tests

### 4. Run Project Setup

Auto-detect and run appropriate setup. **Skip Python install if .venv is symlinked.**

```bash
# Node.js
if [ -f package.json ]; then npm install; fi

# Rust
if [ -f Cargo.toml ]; then cargo build; fi

# Python - skip if .venv is symlinked
if [ -L .venv ]; then
  echo "Using symlinked .venv, skipping Python dependency install"
elif [ -f pyproject.toml ]; then
  uv sync || poetry install
elif [ -f requirements.txt ]; then
  pip install -r requirements.txt
fi

# Go
if [ -f go.mod ]; then go mod download; fi
```

### 5. Verify Clean Baseline

Run tests to ensure worktree starts clean:

```bash
# Examples - use project-appropriate command
npm test
cargo test
pytest
go test ./...
```

**If tests fail:** Report failures, ask whether to proceed or investigate.

**If tests pass:** Report ready.

### 6. Report Location

```
Worktree ready at <full-path>
Tests passing (<N> tests, 0 failures)
Ready to implement <feature-name>
```

## Quick Reference

| Situation | Action |
|-----------|--------|
| `worktrees/` exists | Use it (verify ignored) |
| No directory exists | Check CLAUDE.md → default to `worktrees/` |
| Directory not ignored | Add `worktrees/` to .gitignore + commit |
| `.env` exists in main | Symlink it |
| `.venv` exists in main | Symlink it, skip Python install |
| Tests fail during baseline | Report failures + ask |
| No package.json/Cargo.toml | Skip dependency install |

## Common Mistakes

### Skipping ignore verification

- **Problem:** Worktree contents get tracked, pollute git status
- **Fix:** Always use `git check-ignore` before creating project-local worktree

### Assuming directory location

- **Problem:** Creates inconsistency, violates project conventions
- **Fix:** Follow priority: existing > CLAUDE.md > ask

### Proceeding with failing tests

- **Problem:** Can't distinguish new bugs from pre-existing issues
- **Fix:** Report failures, get explicit permission to proceed

### Hardcoding setup commands

- **Problem:** Breaks on projects using different tools
- **Fix:** Auto-detect from project files (package.json, etc.)

## Example Workflow

```
You: I'm using the using-git-worktrees skill to set up an isolated workspace.

[Check worktrees/ - exists]
[Verify ignored - git check-ignore confirms worktrees/ is ignored]
[Create worktree: git worktree add worktrees/auth -b feature/auth]
[Symlink .env from main worktree]
[Symlink .venv from main worktree]
[Skip Python install - using symlinked .venv]
[Run pytest - 47 passing]

Worktree ready at /Users/jesse/myproject/worktrees/auth
Symlinked: .env, .venv
Tests passing (47 tests, 0 failures)
Ready to implement auth feature
```

## Red Flags

**Never:**
- Create worktree without verifying it's ignored (project-local)
- Skip baseline test verification
- Proceed with failing tests without asking
- Assume directory location when ambiguous
- Skip CLAUDE.md check

**Always:**
- Follow directory priority: existing > CLAUDE.md > ask
- Verify directory is ignored for project-local
- Auto-detect and run project setup
- Verify clean test baseline

## Output

**After completing this skill, you are in the worktree directory.** All subsequent work happens there.

Report:
- Worktree path (full absolute path)
- Symlinked files (if any): .env, .venv
- Test results (N passing, 0 failures)
- Ready status
- Confirm: "Working directory is now `<path>`"

The workflow orchestrator handles what comes next.
