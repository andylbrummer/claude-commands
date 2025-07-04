Based on the following request:

$ARGUMENTS

## Task Planning Guidelines

### File Scope Best Practices
- **Be Explicit**: List exact file paths, not just directory names
- **Minimize Scope**: Each task should touch 3-7 files maximum
- **Group Logically**: Files that change together should be in same task
- **Consider Dependencies**: Order tasks to avoid conflicts

### Risk Reporting Template
```markdown
⚠️ **HIGH RISK**: Authentication changes

**Risks:** Security-critical code, credential exposure, timing attacks
**Mitigation:** OWASP guidelines, comprehensive tests, gradual rollout

Task: Add user authentication endpoint
Risk: HIGH | Files: /src/routes/auth.js, /src/models/User.js, /tests/auth.test.js
Criteria:
- [ ] POST /api/auth/login: 200 (valid) / 401 (invalid)
- [ ] Run: `npm test tests/auth.test.js`
- [ ] Security: No password logs, timing safe
```

## Task Workflow

### 1. Analysis & Plan Review
- Analyze request and search relevant codebase files
- **Risk Assessment**: Technical, integration, breaking changes, performance, security
- Create plan in `tasks/todo.md` with risk levels, file paths, validation commands
- **Order by risk**: High-risk items first
- **Multiple Approaches**: Evaluate 2-3 strategies, recommend best with reasoning
- **⚠️ Report High Risks**: Security, database, API, performance, breaking changes - get approval first
- **Task Definitions**: Risk level, file paths, validation commands, success criteria
- **Wait for approval** before implementation

### 2. Setup Git Branch
- Create feature branch after approval: `git checkout -b feature/<task-name>`

### 3. Implementation
- **Start with highest risk tasks first** to discover blockers early
- **High-risk tasks**: Extra care, comprehensive error handling, extensive tests, minimal PoC
- Work through todo items systematically, one at a time
- Mark items as `[x]` when completed
- **Before marking complete, verify objective criteria**:
  - Run the specific validation commands listed in the task
  - Confirm expected outputs match actual results
  - Document any deviations from expected outcomes
- **TDD**: Write tests first, ensure they pass before completion
- **Commit after each completed task**:
  ```bash
  git add <modified files>
  git commit -m "<task description>: <what was changed>"
  ```
- Provide concise progress updates after each significant change
- **Principles**: Simplicity first, small commits, test coverage, fail fast, test changes

### 4. Task Completion & List Updates
- **Required**: Include current state with each task (what exists, what's missing, current status)
- **Expected**: Update task list at end reflecting actual work completed
- **Success Criteria**: Check off all success criteria when task is completed
- **State Updates**: Update current state section to reflect new reality after task completion
- **Partial Completion**: Adjust remaining tasks based on what was actually finished
- **Scope Discovery**: Add new tasks discovered during implementation
- **Spike Results**: Convert spike findings into concrete implementation tasks
- **Process Improvements**: Add tasks for refactoring, optimization, or technical debt discovered

#### Task Format with Current State:
```markdown
## Task: Add JWT Token Generation
- **Current State**: 
  - ❌ No JWT service exists
  - ❌ Auth service has basic password validation only  
  - ✅ User model has id and email fields
  - ❌ No token expiration handling
- **Success Criteria**:
  - [ ] JWT service created with generateToken method
  - [ ] Auth service integrates JWT generation
  - [ ] Token includes user id, email, expiration
  - [ ] Tests cover token generation and validation

## After Completion - Updated State:
- **Current State**: 
  - ✅ JWT service exists with generateToken method
  - ✅ Auth service integrates JWT generation  
  - ✅ User model has id and email fields
  - ✅ Token expiration handling implemented
- **Success Criteria**:
  - [x] JWT service created with generateToken method
  - [x] Auth service integrates JWT generation
  - [x] Token includes user id, email, expiration
  - [x] Tests cover token generation and validation
```

#### Task List Update Patterns:
```markdown
## Task Completion Review
- ✅ Completed: User authentication endpoint
- 🔄 Partial: Database migration (3 of 5 tables updated)
- 🆕 Discovered: Need password strength validation
- 🔧 Technical Debt: Legacy auth code needs refactoring
- 🧪 Spike Needed: Evaluate Redis for session storage

## Updated Task List:
- [ ] Complete remaining database tables (users, sessions)
  - **Current State**: Users table ✅, Sessions table ❌, Roles table ❌
- [ ] Add password strength validation middleware
  - **Current State**: Basic validation ✅, Strength rules ❌, Error messages ❌
- [ ] Refactor legacy authentication utilities
  - **Current State**: Legacy code identified, modern patterns researched
- [ ] Research spike: Redis vs in-memory sessions
  - **Current State**: Requirements gathered, evaluation criteria defined
- [ ] Update authentication documentation
  - **Current State**: Old docs exist, new JWT flow undocumented
```

### 5. Documentation & Archival
- Add "## Review" to `todo.md`: changes summary, challenges, improvements
- Archive `todo.md` to `/archive/` with timestamp and description
