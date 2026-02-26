# Breaking Changes Detection Guide

## What is a Breaking Change?

A breaking change is any modification that:

- Removes or renames functionality users depend on
- Changes API contracts (parameters, return values)
- Modifies expected behavior in ways that break existing integrations

## Detection Patterns

### 1. File Deletions

Check for deleted files, especially:

- API endpoint files
- Configuration files
- Public utility functions
- Database migrations

```bash
git diff --name-status | grep "^D"
```

### 2. API Changes

Look for modifications in:

- `routes/`, `api/`, `endpoints/`
- Function signatures (parameter changes)
- Response format changes
- HTTP method changes

**Indicators:**

- `app.get()` → `app.post()` (method change)
- Function parameter added/removed
- Return type changed
- Required parameter added

### 3. Configuration Changes

Check `config/` for changes affecting:

- Environment variables
- Feature flags
- Default values
- Required fields

### 4. Database Schema

In `migrations/`, `schema/`:

- Column removal/rename
- Type changes
- Constraint additions
- Index changes

### 5. Deprecation Patterns

Search for:

- `@deprecated` comments
- Deprecation warnings in code
- TODO comments about removal
- Old version compatibility code

### 6. Type/Signature Changes

In typed files (.ts, .java, etc.):

- Removed/renamed exported functions/classes
- Changed function parameters
- Changed return types
- Changed class members

## Keyword Detection

Search these patterns in diffs:


| Keyword           | Context            | Likely Breaking? |
| ----------------- | ------------------ | ---------------- |
| `delete`          | File, function     | Yes              |
| `remove`          | Feature, parameter | Yes              |
| `drop`            | Table, column      | Yes              |
| `rename`          | File, function     | Yes              |
| `BREAKING`        | Any                | Yes              |
| `deprecated`      | Any                | Future           |
| `!`               | After type/scope   | Yes              |
| `breaking change` | Footer             | Yes              |

## Analysis Steps

### Step 1: Get Full Diff

```bash
git diff --staged
git diff
```

### Step 2: Categorize Changes

- Files added? → Usually safe
- Files modified? → Analyze carefully
- Files deleted? → Likely breaking
- Files renamed? → Check imports

### Step 3: Check Dependencies

If file X changed:

- Who imports X?
- Who uses X's output?
- What tests cover X?

### Step 4: Scan for Patterns

Look for:

- API route changes
- Config key changes
- Function signature changes
- Schema changes

### Step 5: Determine Impact

Ask:

- Will this work with old client code?
- Will old configurations still work?
- Will old database schemas work?

## Decision Tree

```
Change detected
    │
    ▼
Is it a new feature only? ───Yes───► NOT BREAKING
    │
    No
    ▼
Is it adding optional functionality? ───Yes───► LIKELY NOT BREAKING
    │
    No
    ▼
Does it remove/rename anything? ───Yes───► BREAKING
    │
    No
    ▼
Does it change behavior? ───Yes───► CHECK COMPATIBILITY
    │
    No
    ▼
NOT BREAKING
```

## Handling Breaking Changes

1. **Flag clearly** - Use `BREAKING CHANGE:` in footer
2. **Provide migration** - Include how to migrate in body
3. **Version bump** - Major version if not already
4. **Document** - Update docs/changelog

## Example Detection

### Scenario: Modified API endpoint

```diff
- app.get('/api/users/:id', getUser)
+ app.get('/api/users/:userId', getUserByUserId)
```

**Detection:**

- File: `routes/users.js`
- Pattern: Route parameter changed from `id` to `userId`
- **BREAKING** - Old clients using `/api/users/123` will fail

### Scenario: New feature added

```diff
+ function calculateDiscount(price, userType) {
+   if (userType === 'premium') return price * 0.9;
+   return price;
+ }
```

**Detection:**

- File: `utils/pricing.js`
- Pattern: New function added
- **NOT BREAKING** - Only adds functionality

## Summary Checklist

- [ ]  Any deleted files?
- [ ]  Any renamed files/functions?
- [ ]  API routes changed?
- [ ]  Function signatures changed?
- [ ]  Config keys changed?
- [ ]  Database schema changed?
- [ ]  Deprecation added?
- [ ]  Breaking keyword found?

If ANY checked → Mark as BREAKING CHANGE
