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

## Setup Git Branch & Execution Log
```bash
# Create feature branch after approval
git checkout -b feature/<task-name>

# Initialize execution log
cat > execution-log.md << 'EOF'
# Execution Log: [Task Name]
**Started**: [Date/Time]
**Branch**: feature/[task-name]
**Status**: IN_PROGRESS

## File Changes Tracking
### Estimated vs Actual Files
**Estimated Files** (from planning):
- [/path/to/file1.ts] - [Modify]
- [/path/to/file2.ts] - [Create]
- [/path/to/file3.test.ts] - [Create]

**Actual Files** (updated during execution):
- [/path/to/file1.ts] - [Modify] ✅
- [/path/to/file2.ts] - [Create] ✅
- [/path/to/file3.test.ts] - [Create] ✅

### Unexpected Files
- [/path/to/unexpected.ts] - [Reason: discovered during implementation]

## Web Searches Performed
[Track all web searches for research and troubleshooting]

## Build Failures & Fixes
[Track all build failures and their resolutions]

## Multi-Fix Files
[Track files that required 2+ separate fixes]

## Deferred Items
[Track any items pushed to future tasks]

## New Tasks Added
[Track any new tasks discovered during execution]

## Completion Status
- [ ] All estimated files handled
- [ ] All build failures resolved
- [ ] All multi-fix files completed
- [ ] All deferred items documented
- [ ] Log reviewed and formatted
EOF
```

## Implementation Process

### Core Execution Principles
- **Start with highest risk tasks first** to discover blockers early
- **High-risk tasks**: Extra care, comprehensive error handling, extensive tests, minimal PoC
- Work through todo items systematically, one at a time
- Mark items as `[x]` when completed
- **MANDATORY**: Update execution-log.md throughout implementation

### Execution Logging Requirements

#### Log All Web Searches
```bash
# Add to execution-log.md after each web search
echo "
## Web Searches Performed
- **$(date)**: Searched for \"[search terms]\"
  - **Purpose**: [Why you searched]
  - **Key findings**: [What you learned]
  - **Applied**: [How it influenced implementation]
" >> execution-log.md
```

#### Log All Build Failures
```bash
# Add to execution-log.md after each build failure
echo "
## Build Failures & Fixes
- **$(date)**: Build failed with: [error message]
  - **Command**: \`[command that failed]\`
  - **Root cause**: [Why it failed]
  - **Fix applied**: [What you did to fix it]
  - **Resolution time**: [how long it took]
" >> execution-log.md
```

#### Track Multi-Fix Files
```bash
# Add to execution-log.md when a file needs multiple fixes
echo "
## Multi-Fix Files
- **[file path]**: Required [X] separate fixes
  - **Fix 1**: [Description and reason]
  - **Fix 2**: [Description and reason]
  - **Pattern**: [Why multiple fixes were needed]
" >> execution-log.md
```

#### Track File Changes
```bash
# Update execution-log.md as you work on files
update_file_tracking() {
    local file_path="$1"
    local action="$2"  # Modify/Create/Delete
    
    # Mark estimated file as complete
    sed -i "s|- ${file_path} - \[${action}\]|- ${file_path} - \[${action}\] ✅|" execution-log.md
    
    # Or add unexpected file
    if ! grep -q "$file_path" execution-log.md; then
        echo "- ${file_path} - [${action}] ⚠️ UNEXPECTED" >> execution-log.md
    fi
}
```

#### Log Deferred Items
```bash
# Add to execution-log.md when deferring work
echo "
## Deferred Items
- **$(date)**: [Item description]
  - **Reason**: [Why deferred]
  - **Future task**: [When/how it should be addressed]
  - **Priority**: [High/Medium/Low]
" >> execution-log.md
```

#### Log New Tasks Discovered
```bash
# Add to execution-log.md when discovering new tasks
echo "
## New Tasks Added
- **$(date)**: [Task description]
  - **Discovery context**: [When/why discovered]
  - **Estimated effort**: [Time/complexity]
  - **Priority**: [High/Medium/Low]
  - **Dependencies**: [What it depends on]
" >> execution-log.md
```

### Before Marking Task Complete
- Run the specific validation commands listed in the task
- Confirm expected outputs match actual results
- Document any deviations from expected outcomes
- **MANDATORY**: Update execution-log.md completion status

### Test-Driven Development
- **TDD**: Write tests first, ensure they pass before completion

### Commit After Each Completed Task
```bash
git add <modified files>
git commit -m "<task description>: <what was changed>"

# Update execution log with commit
echo "- **$(date)**: Committed [description]" >> execution-log.md
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

#### Final Execution Log Update
```bash
# Complete the execution log before review
cat >> execution-log.md << 'EOF'

## Final Summary
**Completed**: [Date/Time]
**Status**: COMPLETE
**Branch**: feature/[task-name]

### File Changes Summary
**Total Estimated Files**: [X]
**Total Actual Files**: [Y]
**Variance**: [+/-Z files]

### Accuracy Assessment
- **Scope Accuracy**: [How accurate was the initial scope?]
- **Time Accuracy**: [Actual vs estimated time]
- **Complexity Accuracy**: [Was complexity assessment correct?]

### Key Learnings
- **Process Improvements**: [What could be done better next time]
- **Planning Insights**: [What was missed in planning]
- **Technical Discoveries**: [New technical insights gained]

### Handoff Notes
- **Review Focus**: [Areas needing extra attention in review]
- **Risk Areas**: [Parts of implementation that need careful review]
- **Testing Notes**: [Specific testing requirements or concerns]
EOF

# Commit execution log
git add execution-log.md
git commit -m "docs: complete execution log for task"
```

## MANDATORY: Task Completion Results and Handoff

### Critical Completion Requirements

**⚠️ IMPORTANT**: When the execution phase completes, you MUST provide the user with explicit verification of results before any handoff. Future agents will start with ZERO context from this execution session.

#### 1. Exact File Results Verification (REQUIRED)
```bash
# MANDATORY: List all files created/modified with verification commands
echo "=== EXECUTION RESULTS VERIFICATION ==="
echo "Files created/modified during this execution:"

# List actual files changed with git status
git status --porcelain

# For each created/modified file, provide verification:
echo "\n=== FILE VERIFICATION COMMANDS ==="
echo "User can verify these results with:"

for file in $(git diff --name-only HEAD~1); do
    echo "📄 File: $file"
    echo "   Verify exists: ls -la '$file'"
    echo "   View changes: git diff HEAD~1 '$file'"
    echo "   Content check: head -20 '$file'"
    echo ""
done
```

#### 2. Results Summary for User Review (REQUIRED)
```markdown
## EXECUTION COMPLETION SUMMARY

### Files Created/Modified
**Location**: [Provide absolute paths]
- `/absolute/path/to/file1.ext` - [Description of what was created/modified]
- `/absolute/path/to/file2.ext` - [Description of what was created/modified] 
- `/absolute/path/to/file3.ext` - [Description of what was created/modified]

### Verification Commands
**User can verify results with these commands:**
```bash
# Check all modified files
git status

# View specific file contents
cat /absolute/path/to/file1.ext
cat /absolute/path/to/file2.ext

# Verify functionality works
[Include specific test/run commands]
```

### Current State Summary
**What exists now:**
- ✅ [Describe current state of each component]
- ✅ [What functionality is working]
- ⏳ [What still needs work, if any]

### Next Steps Required
1. **Immediate**: [What user should do next]
2. **Review**: [Specific areas to review]
3. **Testing**: [How to test the implementation]
4. **Integration**: [Next integration steps needed]

### For Future Agents
**⚠️ CRITICAL**: Any future agent will need this context:
- **Current Implementation**: [Brief description of what was built]
- **Key Files**: [List most important files with absolute paths]
- **Approach Used**: [Brief description of technical approach]
- **Outstanding Issues**: [Any issues that need resolution]
- **Next Development Phase**: [What comes next in development]
```

#### 3. Branch and Git State Documentation (REQUIRED)
```bash
# MANDATORY: Document exact git state for handoff
echo "=== GIT STATE DOCUMENTATION ==="
echo "Current branch: $(git branch --show-current)"
echo "Latest commit: $(git log -1 --oneline)"
echo "Total commits in this session: $(git rev-list --count HEAD ^origin/$(git branch --show-current) 2>/dev/null || echo 'N/A')"
echo "Branch status: $(git status -s | wc -l) files changed"

echo "\n=== BRANCH VERIFICATION ==="
echo "User can verify branch state with:"
echo "git branch -v"
echo "git log --oneline -5"
echo "git diff origin/$(git branch --show-current)" 
```

#### 4. Context Preservation for Future Sessions (REQUIRED)
```markdown
## CONTEXT PRESERVATION CHECKLIST

### For User Reference
- [ ] **File Locations Documented**: All created/modified files listed with absolute paths
- [ ] **Verification Commands Provided**: User can independently verify results
- [ ] **Current State Described**: Clear description of what exists now
- [ ] **Next Steps Defined**: Clear guidance on what to do next
- [ ] **Issues Documented**: Any problems or limitations noted

### For Future Agent Sessions
- [ ] **Implementation Approach Documented**: Technical approach clearly explained
- [ ] **Key Files Identified**: Most important files highlighted with purposes
- [ ] **Outstanding Work Defined**: What still needs to be done
- [ ] **Integration Points Noted**: How this connects to other components
- [ ] **Testing Strategy Documented**: How to verify functionality
```

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