# Angular Commits Format

## Overview

The Angular Commit Message Format is a standardized structure for git commit messages that enables:

- Automated changelog generation
- Semantic versioning automation
- Clear communication of changes

## Format Structure

```
<type>(scope): <description>

[body]

[optional footer(s)]
```

## Components

### 1. Type (Required)


| Type       | Description                             |
| ---------- | --------------------------------------- |
| `feat`     | New feature                             |
| `fix`      | Bug fix                                 |
| `docs`     | Documentation only                      |
| `style`    | Code style (formatting, semicolons)     |
| `refactor` | Code change that neither fixes nor adds |
| `perf`     | Performance improvement                 |
| `test`     | Adding or updating tests                |
| `build`    | Build system or dependencies            |
| `ci`       | CI configuration                        |
| `chore`    | Other changes (maintenance)             |
| `revert`   | Revert previous commit                  |
| `agents`   | AI functionality (Rules/Skills)         |

### 2. Scope (Required)

The scope provides additional context. Always include it:

- `feat(auth):` - authentication feature
- `fix(api):` - API related fix
- `docs(readme):` - readme documentation

### 3. Description (Required)

- Use imperative mood: "add" not "added" or "adds"
- Keep under 50 characters
- No period at end
- Lowercase first letter

### 4. Body (Required)

- Separate from header with blank line
- Use imperative mood
- Explain "what" and "why", not "how"
- Wrap at 72 characters

**Note:** If you use a Keep-a-Changelog style body (### Added/Changed/Fixed), bullets are often written in **past tense**. That's acceptable—keep the **header** imperative.


### 5. Footer (Optional)

Used for:

- Breaking changes: `BREAKING CHANGE: description`
- Issue references: `Closes #123`, `Fixes #456`
- Co-authors: `Co-authored-by: name <email>`

## Breaking Changes

### In Footer

```
BREAKING CHANGE: api endpoint /users now returns JSON instead of XML
```

### In Description

```
feat(api)!: change users endpoint response format
```

The `!` after type/scope indicates breaking change.

## Examples

### Simple

```
feat: add user login functionality
```

### With Scope

```
feat(auth): add OAuth2 login support
```

### With Body

```
feat(auth): add password reset functionality

- Send reset email with 15-minute expiration
- Store reset token securely in database
- Add rate limiting to prevent abuse

Closes #123
```

### Breaking Change

```
feat(api)!: change user response format

BREAKING CHANGE: /api/users now returns `{ id, name, email }`
instead of `{ userId, fullName, emailAddress }`

Migration guide:
- Update client code to use new field names
- Run database migration to rename columns
```

## Best Practices

1. **One logical change per commit** - Split unrelated changes
2. **Imperative mood** - "add feature" not "added feature"
3. **Be specific** - "fix login validation bug" not "fix bugs"
4. **Use body for context** - Explain why, not how
5. **Reference issues** - Link to related issues/tickets
