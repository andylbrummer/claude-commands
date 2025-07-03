Based on the following request:

$ARGUMENTS

## Master Request Workflow

### 1. Request Analysis & PRD Creation

#### Task Master Codebase Mapping Phase
**CRITICAL**: Before creating any tasks, the Task Master MUST:
1. **Deep Codebase Analysis**: Read relevant files, map change surface, identify integration points, document dependencies

2. **File Mapping Document**: Create `requests/<request-name>/file-mapping.md`:
   ```markdown
   # Codebase File Mapping
   
   ## Change Points Identified
   - Primary files requiring modification
   - Secondary files with dependencies
   - Test files that need updates
   - Configuration files affected
   
   ## File Dependency Graph
   - Shows relationships between files
   - Identifies circular dependencies
   - Maps data flow paths
   
   ## Integration Boundaries
   - API contracts between modules
   - Shared types and interfaces
   - External service touchpoints
   ```

3. **Task Decomposition**: 3-5 files per task, 15-30 min completion, logical grouping, testable outcomes

- Create master request folder: `requests/<request-name>/`
- Generate comprehensive PRD (Product Requirements Document) in `requests/<request-name>/prd.md`:
  ```markdown
  # Product Requirements Document
  
  ## Overview
  - Request summary
  - Business context
  - Success criteria
  
  ## Goals & Objectives
  - Primary goals
  - Secondary objectives
  - Non-goals (out of scope)
  
  ## High-Level Plan
  - Architecture overview
  - Component breakdown
  - Integration points
  
  ## Constraints
  - Technical constraints
  - Time constraints
  - Resource constraints
  - Dependencies
  
  ## Risk Assessment
  - Technical complexity, integration points, security vulnerabilities
  - Performance bottlenecks, breaking changes
  - Mitigation strategies
  
  ## Task Breakdown
  - Ordered task list by risk (high-risk first)
  - Risk level for each task (High/Medium/Low)
  - Dependencies between tasks
  - Estimated effort per task
  - Persona assignments
  - Mitigation strategies for high-risk items
  ```

### 2. Task Generation & Persona Assignment
Create individual task files in `requests/<request-name>/tasks/`:

#### Task Types & Personas:
- **Research** (`01-research-*.md`) → Research Analyst: Tech evaluation, best practices
- **Design** (`02-design-*.md`) → System Architect: Architecture, API, data models  
- **Development** (`03-dev-*.md`) → Software Engineer: Implementation, refactoring, fixes
  - **Auto-followed by**: Review + Test tasks
- **Test Development** (`04-test-*.md`) → QA Engineer: Unit/integration test creation
- **Code Review** (`05-review-*.md`) → Senior Engineer: Quality, performance, security
- **Testing** (`06-testing-*.md`) → Test Engineer: Execution, regression, UAT
- **Integration** (`07-integration-*.md`) → DevOps Engineer: Service integration, config
- **Deployment** (`08-deploy-*.md`) → Release Engineer: Deployment, procedures, rollback

#### Development Task Cycle
**Development Cycle**: Pre-test → Dev → Review → Post-test
- **Failures**: Reset git, add feedback, retry (max 2 cycles)

#### Task File Template:
```markdown
# Task: <Task Title>

## Persona: <Assigned Persona>
Role: <Role Description>
Expertise: <Required Skills>

## File Scope Definition
**Explicit file list for this task:**
```yaml
read_files:   [/path/to/file1.ts, /path/to/file2.ts, /tests/existing.test.ts]
modify_files: [/path/to/file1.ts, /path/to/file2.ts]
create_files: [/tests/file1.test.ts, /docs/feature.md]
# Total: 6 files (keep under 10)
```

## Task Requirements
- Objective: <Clear task objective>
- Risk Level: <HIGH/MEDIUM/LOW>
- Risk Factors: <What makes this risky?>
- Dependencies: <List of prerequisite tasks>
- Deliverables: <Expected outputs>
- Acceptance Criteria: <Definition of done>

## Risk Mitigation
- Spike Tasks: <Any PoC needed before full implementation?>
- Validation Steps: <Extra checks for high-risk items>
- Rollback Plan: <How to undo if something goes wrong>
- Progressive Rollout: <Can this be feature-flagged?>

## Success Validation
```bash
npm test path/to/specific/test.ts           # Tests pass
npm run typecheck -- path/to/file1.ts      # No type errors  
npm run lint path/to/file1.ts               # No lint warnings
grep -q "methodName" path/to/file1.ts       # Method implemented
npm run test:integration -- --grep "feature" # Integration pass
```

## Context from PRD
<Relevant sections from master PRD>

## Language-Specific Reminders
<If applicable, language/framework specific guidelines>

## Constraints
- Time: <Estimated hours>
- Resources: <Required tools/access>
- Technical: <Technical limitations>
- Token Budget: <15,000 tokens recommended>

## Past Failures & Lessons Learned
<Historical context if this is a retry or similar task>

## Retry Information (Development Tasks Only)
- Attempt: <1, 2, or FINAL>
- Previous Failures: <List of what went wrong>
- Feedback from Review: <Specific issues found>
- Feedback from Testing: <Test failures and reasons>

## Git Worktree
- Branch: `task/<task-id>-<short-description>`
- Base: `feature/<request-name>`
- Reset Strategy: `git reset --hard origin/feature/<request-name>` (if retry)

## Scope Expansion (Review/Validation Only)
- Document missing files, request expansion with justification, update file-mapping.md

## Checklist
- [ ] Review requirements
- [ ] Verify all files in scope exist
- [ ] Set up worktree
- [ ] Implement solution within file scope
- [ ] Run all validation commands
- [ ] Ensure all validations pass
- [ ] Update documentation
- [ ] Create PR for review
```

### 3. Git Workflow Management

#### Git Setup:
```bash
git checkout -b feature/<request-name> && git push -u origin feature/<request-name>
git worktree add -b task/<task-id>-<description> ../worktrees/<task-id> feature/<request-name>
```

#### Execution Process:
1. Assign task to persona → Create worktree → Pre-test (dev tasks)
2. Execute with subagent → Track via git commits
3. Auto review/test cycle → Retry on failure (max 2x) → Integrate

#### Flow: 
Pre-Test → Dev → Review → Post-Test (with reset/retry on failure)

### 4. Task Master Responsibilities

The Task Master (orchestrator) will:

#### Phase 1: Deep Codebase Understanding
**Critical**: Task Master must understand codebase upfront:
1. **File Discovery**: Use Glob patterns, analyze structures, map relationships, document patterns

2. **Change Impact Analysis**: Identify modification files, map ripple effects, find dependencies

3. **Task Decomposition**: 3-5 file scopes, risk-ordered (high first), spike tasks for risky integrations, no file conflicts, logical grouping

#### Phase 2: Task Creation & Management
- Parse initial request and create PRD
- Generate file-mapping.md with complete codebase understanding
- Break down work into atomic, file-scoped tasks
- Assign appropriate personas
- Manage task dependencies
- Monitor progress via git
- Handle task failures and retries
- Coordinate integration

#### Principles:
1. **Deep Expert**: Task Master reads extensively, subagents get explicit scopes
2. **Small Tasks**: 15-30 min chunks, <10 files, with validation criteria

#### Review/Validation Rules:
- Review/Testing tasks may expand scope, document expansions, update file-mapping.md

#### Task Status Tracking:
```markdown
## Status Board
| Task ID | Title | Persona | Status | Attempt | Worktree | PR |
|---------|-------|---------|--------|---------|----------|-----|
| 01-research-api | API Research | Research Analyst | Completed | 1 | task/01-research-api | #12 |
| 02-design-schema | Schema Design | System Architect | Completed | 1 | task/02-design | #13 |
| 06-pre-test-auth | Pre-Test: Auth | Test Engineer | Completed | 1 | task/06-pre-test-auth | - |
| 03-dev-auth | Auth Implementation | Software Engineer | In Progress | 2 | task/03-dev-auth | - |
| 05-review-auth | Review: Auth | Senior Engineer | Pending | - | - | - |
| 06-post-test-auth | Post-Test: Auth | Test Engineer | Pending | - | - | - |

## Failure Tracking
| Task ID | Attempt | Failure Type | Feedback Summary |
|---------|---------|--------------|------------------|
| 03-dev-auth | 1 | Code Review | Missing error handling, incorrect types |
```

### 5. Integration & Completion

#### Integration:
```bash
git merge task/<task-id>-<description> && git worktree remove ../worktrees/<task-id>
```

#### Final Review:
- Consolidate all task outputs
- Update master PRD with outcomes
- Create integration test suite
- Prepare deployment artifacts

#### Archive Request:
```bash
# Move completed request to archive
mv requests/<request-name>/ archive/requests/$(date +%Y%m%d)-<request-name>/
git add archive/
git commit -m "Archive completed request: <request-name>"
```

### 6. Monitoring & Reporting

Generate progress reports in `requests/<request-name>/reports/`:
- Daily status updates
- Dependency tracking
- Risk assessments
- Blockers and mitigation

### 7. Example Usage

```bash
# Initialize a new master request
claude-code "Create a comprehensive authentication system with OAuth2, JWT tokens, and user management"

# This will:
# 1. Create requests/auth-system/ folder
# 2. Task Master performs deep codebase analysis (reads 50+ files)
# 3. Generate file-mapping.md with all touchpoints
# 4. Generate PRD with full requirements
# 5. Create 20+ small, file-scoped task files
# 6. Each task specifies exact files to read/modify/create
# 7. Each task includes validation commands
# 8. Set up git worktrees for parallel development
# 9. Assign personas and track progress
```

### 8. Pre-Test, Review, and Post-Test Task Templates

#### Pre-Development Test Task Template:
```markdown
# Task: Pre-Development Tests for <Dev Task Name>

## Persona: Test Engineer
Role: Baseline Tester
Expertise: System validation, regression testing

## Purpose
Establish system baseline before development changes to ensure:
- Existing functionality works correctly
- No pre-existing failures that could be blamed on new code
- Clear before/after comparison possible

## File Scope Definition
```yaml
# Focus on area that will be modified
test_scope:
  - Tests related to <feature area>
  - Integration tests for affected modules
  - Any tests that touch files in dev task scope

baseline_files:
  <list files from upcoming dev task>
```

## Baseline Validation
```bash
# Run all unit tests for feature area
npm test <feature-area>/**/*.test.ts
# Expected: All tests pass (document any existing failures)

# Run integration tests
npm run test:integration -- --grep "<feature-area>"
# Expected: All tests pass

# Type checking for files to be modified
npm run typecheck -- <files-to-be-modified>
# Expected: No type errors

# Linting check
npm run lint <files-to-be-modified>
# Expected: No lint errors

# Coverage baseline
npm run coverage -- <feature-area>
# Record: Current coverage percentage
```

## Baseline Report
- [ ] All relevant tests passing
- [ ] No type errors in target files
- [ ] No lint issues in target files
- Current test coverage: ____%
- Existing issues found: <list any>

## Go/No-Go Decision
- [ ] PROCEED: System stable, ready for development
- [ ] STOP: Fix existing issues first (document below)
```

### 9. Code Review and Post-Test Task Templates

#### Code Review Task Template:
```markdown
# Task: Code Review for <Dev Task Name>

## Persona: Senior Engineer
Role: Code Reviewer
Expertise: Code quality, best practices, security

## Review Scope
- Dev Task: <task-id>
- Worktree: task/<dev-task-id>
- PR: <pr-number>

## File Scope Definition
```yaml
# Expanded scope allowed for review
review_files:
  <all files from dev task>
  
search_patterns:
  - "TODO|FIXME|XXX"  # Find incomplete work
  - "console\\.log"   # Debug statements
  - "any\\s*\\["      # Type safety issues
```

## Review Checklist
- [ ] Code follows project conventions
- [ ] Error handling is comprehensive
- [ ] Types are properly defined
- [ ] No security vulnerabilities
- [ ] Performance considerations addressed
- [ ] Tests cover edge cases
- [ ] Documentation updated

## Success Validation
```bash
# Run linting on changed files
npm run lint -- <modified files>

# Check test coverage
npm run coverage -- <test files>

# Security scan
npm audit
```

## Review Outcome
- [ ] PASS: Ready to merge
- [ ] FAIL: Needs revision (document issues below)

## Feedback for Retry
<If failed, specific actionable feedback>
```

#### Post-Development Test Task Template:
```markdown
# Task: Post-Development Testing for <Dev Task Name>

## Persona: Test Engineer  
Role: Integration Tester
Expertise: End-to-end testing, system integration

## Test Scope
- Dev Task: <task-id>
- Feature: <feature-name>

## File Scope Definition
```yaml
# Broader scope for testing
test_files:
  - All test files from dev task
  - Related integration test suites
  
execute_tests:
  - Unit tests for modified files
  - Integration tests for feature
  - E2E tests if applicable
```

## Test Execution Plan
1. Run unit tests for new/modified code
2. Run integration tests for feature area
3. Run regression tests for related features
4. Verify no breaking changes

## Success Validation
```bash
# Unit tests
npm test <specific test files>

# Integration tests
npm run test:integration -- --grep "<feature>"

# E2E tests (if applicable)
npm run test:e2e -- --spec "<feature>"

# Regression check
npm run test:regression
```

## Test Results
- [ ] All new tests pass
- [ ] No regression failures
- [ ] Performance benchmarks met
- [ ] Integration points verified

## Feedback for Retry
<If failed, specific test failures and root causes>
```

### 10. Example Development Task Sequence

For a task "Add JWT Token Generation" (HIGH RISK - Security Critical), the complete sequence would be:

1. **03-spike-jwt**: Proof of concept for JWT library integration (30 min)
2. **06-pre-test-jwt**: Verify auth system tests pass
3. **03-dev-jwt**: Implement JWT token generation  
4. **05-review-jwt**: Security-focused code review
5. **06-post-test-jwt**: Verify all tests + security validation
6. **03-security-jwt**: Additional security hardening if needed

### 11. Example Task with File Scope

```markdown
# Task: Add JWT Token Generation

## Persona: Software Engineer
Role: Backend Developer
Expertise: Node.js, JWT, TypeScript

## File Scope Definition
```yaml
read_files:
  - /src/config/auth.config.ts  # Auth configuration
  - /src/types/user.types.ts    # User type definitions
  - /src/utils/crypto.utils.ts  # Existing crypto utilities

modify_files:
  - /src/services/auth.service.ts  # Add generateToken method

create_files:
  - /src/services/jwt.service.ts      # New JWT service
  - /src/services/jwt.service.test.ts # JWT service tests
  - /src/types/jwt.types.ts          # JWT type definitions

# Total: 7 files (well within token limits)
```

## Success Validation
```bash
# Unit tests pass
npm test src/services/jwt.service.test.ts
# Expected: ✓ 5 passing tests

# Type checking
npm run typecheck -- src/services/jwt.service.ts
# Expected: No errors

# Integration test
npm run test:integration -- --grep "JWT token generation"
# Expected: ✓ Token generated successfully

# Verify method exists
grep -q "generateToken" src/services/auth.service.ts && echo "✓ Method added"
# Expected: ✓ Method added
```
```

## Benefits
- **Parallel Development**: Multiple subagents work on different tasks simultaneously
- **Clear Ownership**: Each task has a dedicated persona with specific expertise
- **Git-based Tracking**: Full history and traceability of all changes
- **Isolation**: Worktrees prevent conflicts between concurrent tasks
- **Scalability**: Can handle multi-day development efforts efficiently
- **Quality Assurance**: Automatic review and testing after each dev task
- **Failure Recovery**: Up to 2 retry cycles with feedback integration
- **Token Efficiency**: Precise file scopes minimize token usage