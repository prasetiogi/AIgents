# Body Format

## Overview

This format provides a human-readable structure for commit message bodies.

## Categories

### Added
New features added.
```
### Added
- User authentication with OAuth2
- Password reset functionality
- API rate limiting
```

### Changed
Changes to existing functionality.
```
### Changed
- Improved API response time by 30%
- Updated user interface colors
- Refactored database queries
```

### Deprecated
Features that will be removed in future releases.
```
### Deprecated
- `getUserById(id)` - use `getUser(id)` instead
- XML response format - JSON only in v2.0
```

### Removed
Features removed in this release.
```
### Removed
- Legacy v1 API endpoints
- Support for Internet Explorer 11
- Deprecated `User.getProfile()` method
```

### Fixed
Bug fixes.
```
### Fixed
- Login validation error with special characters
- Memory leak in image processing
- Race condition in concurrent requests
```

### Security
Security-related changes.
```
### Security
- Patched XSS vulnerability in comment form
- Updated OpenSSL to 3.0.8
- Added CSRF token validation
```

## Usage in Commit Messages

**Note:** Body is **REQUIRED** - always include at least one category.

Apply these categories to group related changes:

```
feat(auth): add OAuth2 login

### Added
- Google OAuth2 authentication
- GitHub OAuth2 authentication
- Session persistence

### Changed
- Simplified login form UI
```

```
fix(api): resolve memory leak

### Fixed
- Fixed connection pool not releasing connections
- Added proper cleanup on request timeout
```

## Guidelines

1. **SKILL.md is code, not docs** - SKILL.md files define AI agent behavior, not user documentation. Use `refactor` or `agents` type.
2. **Use past tense** - "Added" not "Add"
2. **Be specific** - "Fixed login bug" vs "Fixed bug"
3. **One category per change** - Choose the most relevant
4. **Keep bullet points concise** - Expand in body if needed
5. **Sort by importance** - Added/Removed first, then Changed/Fixed

## Example Complete Commit

```
feat(api): add user profile endpoints

### Added
- GET /api/users/{id}/profile
- PUT /api/users/{id}/profile
- Profile image upload support

### Changed
- Profile data now includes avatar URL

### Fixed
- Resolved 404 error when profile doesn't exist

Closes #234
```
