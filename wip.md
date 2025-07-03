As a technical project manager, generate a comprehensive Work-In-Progress (WIP) summary that provides clear visibility into the current state of development.

## Analysis Scope

Analyze the following sources to build a complete picture:

### 1. Git Repository State
- Run `git status` to identify:
  - Modified files (staged and unstaged)
  - Untracked files
  - Current branch and its relation to main/master
- Run `git diff` to understand:
  - Specific changes in each file
  - Lines added/removed
  - Nature of modifications
- Check recent commits with `git log --oneline -10`

### 2. Project Documentation Review
- **PRD Files**: Look for Product Requirements Documents (PRD.md, requirements.md, specs/)
  - Current implementation status vs requirements
  - Any deviations or scope changes
  - Unimplemented features
- **Task Files**: Search for task lists, TODO files, or project boards
  - dart mcp tasks related to this project
  - Open tasks and their priorities
  - Completed tasks in current sprint/iteration
  - Blocked items or dependencies

### 3. Jira/Issue Tracking
- Referenced ticket numbers in commits or code comments
- Task status and progress
- Related subtasks or epics

### 4. Documentation Changes
- **Requirements Updates**: Compare current requirements with git history
  - New requirements added
  - Modified acceptance criteria
  - Removed or deferred features
- **Technical Documentation**: Check for updates to:
  - Architecture documents
  - API specifications
  - Database schemas
  - Integration guides

## WIP Summary Format

Generate a structured summary with:

### 1. Executive Overview (2-3 sentences)
- Current sprint/iteration goal
- Overall progress percentage
- Key blockers or risks

### 2. Active Development
```
Currently Working On:
- [Feature/Task Name] - [Status] - [% Complete]
  - What's done: Brief description
  - What's left: Remaining work
  - Blockers: Any impediments
```

### 3. Recent Accomplishments (Last 1-2 days)
- Completed features/fixes with commit references
- Resolved issues or bugs
- Documentation updates

### 4. Git Repository Status
```
Branch: [branch-name]
Status: [X files modified, Y untracked]
Key Changes:
- path/to/file: Description of changes
- path/to/file2: Description of changes
```

### 5. Requirements Alignment
```
PRD Compliance:
- ✅ Implemented: [List of completed requirements]
- 🔄 In Progress: [Requirements being worked on]
- ❌ Not Started: [Pending requirements]
- ⚠️  Deviations: [Any changes from original spec]
```

### 6. Task Status
```
Sprint Tasks:
- High Priority: X completed / Y total
- Medium Priority: X completed / Y total
- Low Priority: X completed / Y total

Blocked Tasks:
- [Task]: [Reason for blockage]
```

### 7. Next Steps (Prioritized)
1. Immediate (Today):
   - Specific task with expected outcome
2. Short-term (This week):
   - Key deliverables
3. Upcoming:
   - Future work preparation

### 8. Risks & Dependencies
- Technical risks or debt accumulated
- External dependencies
- Timeline concerns

### 9. Questions for Stakeholders
- Clarifications needed on requirements
- Priority decisions required
- Resource or scope questions

## Additional Checks
- Test coverage for new features
- Performance impact of changes
- Security considerations
- Breaking changes that need communication

Generate this summary in a clear, scannable format that can be quickly reviewed in standup meetings or status updates.