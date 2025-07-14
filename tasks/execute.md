# Task Execution Guidelines

## Prerequisites Check

### MANDATORY: Before Executing Tasks
```markdown
## Task Preparation Verification ✅
- [ ] **Planning Complete**: Task planning has been completed using [tasks-plan.md](tasks-plan.md)
- [ ] **Todo Files Created**: All required todo.md files have been created and initialized
- [ ] **Context Available**: All necessary context and documentation is accessible
- [ ] **Dependencies Ready**: All prerequisite tasks/subtasks are complete and archived
- [ ] **Environment Setup**: Development environment is properly configured

## Execution Readiness Check
- [ ] **Task Files Present**: Current task files exist and contain proper planning details
- [ ] **Success Criteria Defined**: Clear success criteria and validation commands are specified
- [ ] **Risk Assessment Complete**: Risk level and mitigation strategies are documented
- [ ] **Resource Availability**: Required resources (tools, access, time) are available

## Current Task Status Verification
- [ ] **Task Initialized**: Current task is properly initialized with all required information
- [ ] **OR Execution in Process**: Task execution is already underway with proper tracking
- [ ] **Progress Tracking**: Status tracking mechanism is in place
- [ ] **Validation Commands**: All validation commands are tested and working

**⚠️ STOP**: Do not proceed with execution until current task files are properly created and initialized
```

## Quick Start: "Just Start Coding"

### For Simple Tasks (came here directly):
1. Create feature branch: `git checkout -b feature/task-name`
2. Work through your mental checklist
3. Commit after each logical change: `git commit -m "component: what changed"`
4. Verify with basic testing
5. → Go to [tasks-review.md](tasks-review.md) when done

### For Planned Tasks (came from tasks-plan.md):
1. ✅ You should have a todo.md file with ready-to-execute tasks
2. ✅ Risk level and validation commands should be clear
3. Start with **highest risk tasks first**
4. Follow implementation process below

---

## Setup Git Branch
```bash
# Create feature branch after approval
git checkout -b feature/<task-name>
```

## Implementation Process

### Core Execution Principles
- **Start with highest risk tasks first** to discover blockers early
- **High-risk tasks**: Extra care, comprehensive error handling, extensive tests, minimal PoC
- Work through todo items systematically, one at a time
- Mark items as `[x]` when completed

### Before Marking Task Complete
- Run the specific validation commands listed in the task
- Confirm expected outputs match actual results
- Document any deviations from expected outcomes

### Test-Driven Development
- **TDD**: Write tests first, ensure they pass before completion

### Commit After Each Completed Task
```bash
git add <modified files>
git commit -m "<task description>: <what was changed>"
```

### Core Development Principles
- **Simplicity first**
- **Small commits** 
- **Test coverage**
- **Fail fast**
- **Test changes**

### Progress Updates
- Provide concise progress updates after each significant change

## Emergency/Time Pressure Adaptations

### When Methodology Gets In The Way:
```markdown
## Hotfix/Emergency Mode
- Skip detailed planning, but ALWAYS use feature flags for replacements
- Minimum commits: working state + tests + rollback plan
- Document shortcuts taken for later cleanup
- Still validate with stakeholders before deploying

## Prototype Speed Mode  
- Skip extensive error handling (document technical debt)
- Use hard-coded values if they speed development
- Focus on core functionality only
- Plan cleanup tasks for later phases

## "Good Enough" Criteria by Phase
- **Prototype**: Core use case works, basic error handling
- **MVP**: User workflow complete, production-ready error handling  
- **Production**: Full edge cases, monitoring, documentation
```

## When Tasks Get Stuck

### Quick Troubleshooting:
1. **Blocker** → Simplify scope, ask for help, or create spike task
2. **Too Complex** → Break into 2-3 smaller tasks
3. **Wrong Approach** → Switch to simpler method, document decision
4. **Integration Issues** → Mock dependencies, test in isolation

### Real Example - Feature Flag Implementation:
```bash
# When replacing authentication method
git checkout -b feature/new-auth-system

# 1. Add feature flag first
echo "NEW_AUTH_ENABLED=false" >> .env
# Add flag check in auth middleware

# 2. Implement new auth alongside old
# Keep both systems running

# 3. Test new system with flag enabled
NEW_AUTH_ENABLED=true npm test

# 4. Deploy with flag disabled (old system active)
git commit -m "auth: add new auth system behind feature flag"

# 5. Enable flag in production after verification
# 6. Remove old system in separate commit only after extended parallel period
```

### Handoff to Review Phase:
**When task is complete** → Go to [tasks-review.md](tasks-review.md)

## Task Completion & Updates

### Current State Tracking
- **Required**: Include current state with each task (what exists, what's missing, current status)
- **Expected**: Update task list at end reflecting actual work completed
- **Success Criteria**: Check off all success criteria when task is completed
- **State Updates**: Update current state section to reflect new reality after task completion
- **Partial Completion**: Adjust remaining tasks based on what was actually finished
- **Scope Discovery**: Add new tasks discovered during implementation
- **Spike Results**: Convert spike findings into concrete implementation tasks
- **Process Improvements**: Add tasks for refactoring, optimization, or technical debt discovered

### Task Format with Current State
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

### Task List Update Patterns
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