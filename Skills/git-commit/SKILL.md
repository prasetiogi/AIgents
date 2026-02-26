---
name: git-commit
description: "Professional git commit workflow with intelligent change analysis. Use when: (1) User requests to commit changes, (2) User says 'commit', 'git commit', or 'save changes', (3) Before AI agent does any planning for commit task. Features: automatic logical change detection, breaking change detection, Angular commits format, Keep a Changelog body style, commit preview, and .git/COMMIT.TXT draft."
metadata:
  version: 0.1.0
---
# Git Commit

## Overview

This skill provides a comprehensive workflow for creating professional git commits with intelligent change analysis. It automatically:

- Analyzes all uncommitted changes
- Groups changes into logical commits
- Detects breaking changes
- Generates professional commit messages
- Previews plan before execution

## Trigger

Activate this skill when user:

- Says "commit", "git commit", "commit now"
- Says "save", "save changes", "save modifications"
- Asks to commit uncommitted changes
- Any request to create a git commit

**Important:** Activate BEFORE planning. This skill provides the complete workflow.

## Workflow

### Step 1: Analyze Uncommitted Changes

Run these commands to get full picture:

```bash
# Check for staged and unstaged changes
git status

# Get summary of changes
git diff --stat

# View staged changes
git diff --staged --stat
git diff --staged

# View unstaged changes
git diff --stat
git diff
```

Analyze each changed file:

- File extension
- File path/directory
- Type of change: added, modified, deleted, renamed
- Content changes (for modified files)

### Step 2: Group into Logical Changes

Group files that belong together. Use these heuristics:

**Group by Directory:**

- Files in same folder → likely related
- Example: `src/components/Button.tsx` + `src/components/Button.css`

**Group by Feature:**

- Files implementing same feature
- Example: auth files → authentication feature

**Group by Type:**

- All styles → styling changes
- All tests → test changes
- All configs → configuration changes

**Split Rules:**

- Different feature areas → separate commits
- Different types (feat + fix) → separate commits
- Breaking + non-breaking → separate commits
- More than 5 unrelated files → split

### Step 3: Detect Breaking Changes

For each logical group, check for breaking changes. See [breaking-changes.md](docs/breaking-changes.md) for detailed patterns.

**Quick Check:**

- Any deleted files? → likely breaking
- Any API route changes? → breaking
- Any function signature changes? → breaking
- Any config key changes? → may be breaking
- Deprecation added? → future breaking

**Mark as Breaking if:**

- `BREAKING CHANGE:` found in diff
- `!` after type/scope
- File deletion detected
- API endpoint modified
- Function parameter added/removed
- Configuration schema changed

### Step 4: Draft Commit Messages

For each logical group:

**Header (Angular Commits):**

```
<type>(scope): <description>
```

See [angular-commits.md](docs/angular-commits.md) for types:

- `feat` - new feature
- `fix` - bug fix
- `docs` - documentation
- `style` - formatting
- `refactor` - code restructure
- `perf` - performance
- `test` - tests
- `build` - build system
- `ci` - CI/CD
- `chore` - maintenance
- `agents` - AI functionality (Rules/Skills)

**Body (Keep a Changelog):**

```
### Added
-

### Changed
-

### Fixed
-
```

See [keepachangelog.md](docs/keepachangelog.md) for categories.

**Footer:**

- `BREAKING CHANGE: description` - if breaking
- `Closes #123` - issue references

### Step 5: Preview Commit Plan

Show user the planned commits BEFORE executing. Format:

```
=== COMMIT PLAN ===

[1] Logical Change: <description>
Files:
  - file1.ts (modified)
  - file2.ts (new)
Type: feat
Scope: auth
Breaking: YES/NO

Message:
---
<type>(scope): <description>

<body>

<footer>
---

[2] Logical Change: <description>
...

---
Total: X commit(s)

Proceed? [y/n]
```

### Step 6: Write Commit Message File

After user confirms, write message to `.git/COMMIT.TXT`:

```bash
# For single commit
git commit -F .git/COMMIT.TXT

# For multiple commits, use interactive or loop
```

If multiple commits needed:

1. Write first commit message to `.git/COMMIT.TXT`
2. Execute `git commit -F .git/COMMIT.TXT`
3. Unstage those files
4. Repeat for next logical change

### Step 7: Execute Commit

```bash
# Write and commit
git commit -F .git/COMMIT.TXT

# Verify commit
git log -1 --stat
```

## Example Complete Workflow

**User Input:** "commit"

**Step 1 - Analyze:**

```
$ git status
Changes to be committed:
  - src/auth/login.ts (new)
  - src/auth/middleware.ts (modified)

Changes not staged:
  - src/utils/helpers.ts (modified)
  - README.md (modified)
```

**Step 2 - Group:**

- Group 1: auth files (login.ts, middleware.ts) - authentication feature
- Group 2: helpers.ts - utility fix
- Group 3: README.md - documentation

**Step 3 - Detect Breaking:**

- Group 1: New feature, no breaking changes
- Group 2: Refactor, no breaking changes
- Group 3: Documentation, no breaking changes

**Step 4 - Draft:**

```
[1] feat(auth): add login functionality

### Added
- OAuth2 login support
- Auth middleware

### Changed
- Simplified auth flow
```

```
[2] refactor(utils): extract common helper functions

### Changed
- Moved duplicate code to shared helpers
```

```
[3] docs: update README with installation instructions

### Changed
- Added setup guide
- Updated prerequisites
```

**Step 5 - Preview:** Show to user

**Step 6 - Write:** User approves → write to `.git/COMMIT.TXT`

**Step 7 - Execute:** Run `git commit -F .git/COMMIT.TXT`

## Command Reference


| Command                         | Purpose                  |
| ------------------------------- | ------------------------ |
| `git status`                    | List all changed files   |
| `git diff --stat`               | Summary of changes       |
| `git diff --staged`             | Staged changes           |
| `git diff`                      | Unstaged changes         |
| `git diff <file>`               | Specific file diff       |
| `git commit -F .git/COMMIT.TXT` | Commit with message file |
| `git log -1 --stat`             | Verify last commit       |

## Edge Cases

### No Changes

If `git status` shows no changes, inform user:

```
No uncommitted changes found. Nothing to commit.
```

### Too Many Groups

If more than 5 logical groups, suggest grouping less important changes:

```
Found 8 logical changes. Consider combining documentation updates into one commit?
```

### Merge Conflicts

If merge conflicts exist, do not proceed:

```
Merge conflicts detected. Please resolve conflicts first before committing.
```

### Large Files

Check for large files in diff. Consider gitignore:

```
Large binary file detected. Add to .gitignore?
```

## Resources

### docs/

- [angular-commits.md](docs/angular-commits.md) - Header format reference
- [keepachangelog.md](docs/keepachangelog.md) - Body format reference
- [breaking-changes.md](docs/breaking-changes.md) - Breaking change detection
