As a senior software engineer preparing a professional commit, ensure all changes are production-ready and well-documented.

respect the following instructions if provided. 
<instructions>$ARGUMENTS</instructions>


## Pre-Commit Checklist

Review and complete the following before committing:

### 1. Code Quality
- [ ] Remove all debug statements, console.logs, and test prints
- [ ] Clean up commented-out code and TODOs that are resolved
- [ ] Ensure no hardcoded test data or credentials
- [ ] Verify proper error handling and edge cases

### 2. Documentation Updates
- [ ] **Requirements Document**: Update if features added/changed
- [ ] **Technical Documentation**: Document architectural decisions, API changes
- [ ] **README**: Update setup instructions, dependencies, or usage
- [ ] **Code Comments**: Add/update for complex logic only

### 3. Testing
- [ ] Write/update unit tests for new functionality
- [ ] Add integration tests for API endpoints or workflows
- [ ] Verify all tests pass locally
- [ ] Check test coverage meets project standards
- [ ] Remove any test-only code or fixtures from production files

### 4. Task Management
- [ ] Link relevant Dart/GitHub issues in commit message
- [ ] Update task status and add implementation notes
- [ ] Log time spent if required by project

### 5. Changelog
- [ ] Add entry to CHANGELOG.md following Keep a Changelog format:
  - Added: for new features
  - Changed: for changes in existing functionality
  - Deprecated: for soon-to-be removed features
  - Removed: for now removed features
  - Fixed: for any bug fixes
  - Security: for vulnerability fixes

## Commit Message Format

Generate a commit message following this structure:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- **feat**: New feature for the user
- **fix**: Bug fix for the user
- **docs**: Documentation only changes
- **style**: Formatting, missing semicolons, etc; no code change
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: Code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **build**: Changes that affect the build system or dependencies
- **ci**: Changes to CI configuration files and scripts
- **chore**: Other changes that don't modify src or test files

### Subject Line Rules
- Use imperative mood ("Add feature" not "Added feature")
- Don't capitalize first letter
- No period at the end
- Maximum 50 characters

### Body Guidelines
- Wrap at 72 characters
- Explain WHAT and WHY, not HOW
- Include:
  - Motivation for the change
  - Contrast with previous behavior
  - Side effects or consequences

### Footer
- Reference issues: "Fixes #123", "Closes #456"
- Breaking changes: Start with "BREAKING CHANGE:"
- Co-authors: "Co-authored-by: Name <email>"

### Examples

**Good commit message:**
```
feat(auth): add OAuth2 integration with Google

Implement OAuth2 flow to allow users to sign in with their Google
accounts. This reduces friction in the signup process and leverages
existing user credentials.

- Add GoogleAuthProvider service
- Implement callback handling for OAuth flow
- Store refresh tokens securely in database
- Add configuration for Google client ID/secret

Tests added for all new authentication flows.

Fixes #234
```

**Poor commit message:**
```
fixed bug
```

## Final Steps

1. Review git diff to ensure all changes are intentional
2. Run linter and formatter
3. Verify commit follows project conventions
4. Push and create PR with detailed description

Remember: A well-prepared commit saves hours of debugging and helps future developers (including yourself) understand the codebase evolution.