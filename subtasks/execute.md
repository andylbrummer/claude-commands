# Subtasks Execution Guidelines
## Subagent Coordination with Context Preservation

## Prerequisites Check

### MANDATORY: Before Executing Subtasks
```markdown
## Subtask Planning Verification ✅
- [ ] **Master Planning Complete**: Large feature planning completed using [subtasks-plan.md](subtasks-plan.md)
- [ ] **Context Package Created**: Comprehensive context package exists in `/requests/<feature>/context/`
- [ ] **Task Files Generated**: All individual task files created with context references
- [ ] **Worktree Structure Ready**: Git worktrees set up in `~/work/worktrees/<feature>/`
- [ ] **Dependencies Mapped**: Cross-task dependencies identified and ordered

## Context and Resource Verification
- [ ] **Context Distribution**: Context package accessible to all worktrees
- [ ] **Integration Plan Ready**: Cross-task integration points documented
- [ ] **Risk Mitigation Planned**: High-risk items have specific strategies
- [ ] **Source Validation**: All external sources verified and accessible
- [ ] **Resource Allocation**: Test databases, ports, and other resources allocated per task

## Subagent Coordination Readiness
- [ ] **Task Assignment Ready**: Each task has clear persona assignment and scope
- [ ] **Conflict Prevention Setup**: File contention analysis completed and conflicts prevented
- [ ] **Communication Protocols**: Status reporting and coordination mechanisms in place
- [ ] **Integration Points Defined**: Clear interfaces between parallel tasks
- [ ] **Quality Gates Established**: Validation commands and success criteria defined

**⚠️ STOP**: Do not proceed with subagent execution until context package is complete and coordination infrastructure is ready
```

## Quick Start: Subagent Execution

### For Task Master (Orchestrator):
1. ✅ Context package created from [subtasks-plan.md](subtasks-plan.md)
2. ✅ Task files generated with context references
3. ✅ Worktrees set up in `~/work/worktrees/<feature>/`
4. Start subagent coordination process below

### For Individual Subagents:
1. ✅ Receive task file with complete context package
2. ✅ Validate context sources and patterns
3. Follow execution process for your task type
4. → Report completion for integration review

---

## Subagent Coordination Process

### Task Master Responsibilities

#### 1. Critical Conflict Prevention (MANDATORY FIRST STEP)

##### File Contention Monitoring
```bash
# CRITICAL: Monitor file editing conflicts in real-time
monitor_file_contention() {
    echo "=== File Contention Monitor ==="
    
    # Track which tasks are editing which files
    for task_dir in ~/work/worktrees/<feature>/*/; do
        task_name=$(basename "$task_dir")
        echo "Task: $task_name"
        
        # Check git status for modified files
        if [[ -d "$task_dir/.git" ]]; then
            git -C "$task_dir" status --porcelain | while read status file; do
                echo "  $status $file"
                
                # Check if file is being edited by other tasks
                grep -r "$file" ~/work/worktrees/<feature>/*/task.md 2>/dev/null | \
                    grep -v "$task_dir" && echo "    ⚠️ CONFLICT: File also targeted by other tasks"
            done
        fi
        echo ""
    done
}

# Run every 5 minutes during active development
watch -n 300 monitor_file_contention
```

##### Merge Conflict Prevention Protocol
```markdown
## MANDATORY: Conflict Prevention Rules

### Rule 1: Shared File Coordination
**Before any task starts editing shared files:**
1. **Lock Shared Files**: Create file edit coordination
2. **Version Interfaces**: Create interface extension pattern
3. **Namespace Changes**: Use separate namespaces/sections
4. **Sequential Editing**: High-contention files edited sequentially

### Rule 2: Interface File Management
**For type definition and interface files:**
```bash
# Create interface versioning system
create_interface_version() {
    local base_file="$1"
    local task_id="$2"
    
    # Create task-specific interface extension
    cp "$base_file" "${base_file%.ts}.${task_id}.ts"
    echo "// Extends base interface for task: $task_id" >> "${base_file%.ts}.${task_id}.ts"
    
    # Update task to use extended interface
    echo "Using interface extension: ${base_file%.ts}.${task_id}.ts"
}
```

### Rule 3: Configuration File Coordination
**For configuration and utility files:**
- **Namespace Separation**: Each task gets own config section
- **Merge Strategy**: Use structured merge with conflict markers
- **Validation**: Automated validation of merged configs
```

##### Resource Contention Prevention
```bash
# Prevent database/service contention
allocate_test_resources() {
    local task_id="$1"
    local base_port=3000
    
    # Calculate unique port for each task
    task_number=$(echo "$task_id" | grep -o '[0-9]*' | head -1)
    task_port=$((base_port + task_number * 10))
    
    echo "Task $task_id allocated ports: $task_port-$((task_port + 9))"
    
    # Create task-specific test database
    echo "CREATE DATABASE test_${task_id};" | psql postgres
    
    # Set task-specific environment
    cat > "~/work/worktrees/<feature>/$task_id/.env.task" << EOF
PORT=$task_port
DB_NAME=test_${task_id}
REDIS_PORT=$((task_port + 1))
TEST_PORT=$((task_port + 2))
EOF
}
```

#### 2. Context Package Distribution
```bash
# Ensure all worktrees have context access
for task_dir in ~/work/worktrees/<feature>/*/; do
    ln -sf $(pwd)/requests/<feature>/context $task_dir/context
    echo "Context linked for: $task_dir"
done

# Validate context accessibility
for task_dir in ~/work/worktrees/<feature>/*/; do
    if [[ -f "$task_dir/context/codebase-analysis.md" ]]; then
        echo "✓ Context accessible: $task_dir"
    else
        echo "✗ Context missing: $task_dir"
    fi
done
```

#### 2. Subagent Task Assignment
```markdown
## Task Assignment Process
1. **High-Risk Tasks First**: Start with HIGH risk tasks to discover blockers early
2. **Parallel Safe Tasks**: Launch multiple MEDIUM/LOW risk tasks in parallel
3. **Dependency Ordering**: Ensure prerequisite tasks complete before dependent tasks
4. **Context Validation**: Each subagent validates context before starting

## Subagent Spawn Commands
```bash
# Research tasks (can run in parallel)
claude-code --worktree ~/work/worktrees/<feature>/01-research-api/ "Execute research task with context from ./context/"

# Architecture tasks (depends on research)
claude-code --worktree ~/work/worktrees/<feature>/02-arch-design/ "Execute architecture task with context from ./context/"

# Development tasks (parallel after architecture)
claude-code --worktree ~/work/worktrees/<feature>/03-dev-backend/ "Execute development task with context from ./context/" &
claude-code --worktree ~/work/worktrees/<feature>/03-dev-frontend/ "Execute development task with context from ./context/" &
claude-code --worktree ~/work/worktrees/<feature>/03-dev-integration/ "Execute development task with context from ./context/" &

# Wait for development completion before review
wait
```

#### 3. Progress Monitoring
```markdown
## Task Status Tracking
| Task ID | Persona | Status | Worktree | Context Applied | Issues |
|---------|---------|--------|----------|-----------------|--------|
| 01-research-api | Research Analyst | ✅ Complete | ~/work/worktrees/feature/01/ | ✅ | None |
| 02-arch-design | System Architect | 🔄 In Progress | ~/work/worktrees/feature/02/ | ✅ | None |
| 03-dev-backend | Software Engineer | ⏸️ Blocked | ~/work/worktrees/feature/03/ | ❌ Missing context | Context link broken |
| 03-dev-frontend | Software Engineer | ⏳ Pending | ~/work/worktrees/feature/04/ | - | Waiting for arch |
```

## Individual Subagent Execution

### Context Validation & Execution Log Setup (Required First Step)
```bash
# Every subagent must validate context access
echo "Validating context package..."
if [[ ! -d "./context" ]]; then
    echo "❌ FATAL: Context package not accessible"
    echo "Required: ./context/ directory with master analysis"
    exit 1
fi

# Validate required context files
required_files=("codebase-analysis.md" "external-sources.md" "implementation-patterns.md")
for file in "${required_files[@]}"; do
    if [[ ! -f "./context/$file" ]]; then
        echo "❌ FATAL: Missing required context: $file"
        exit 1
    fi
    echo "✅ Context file present: $file"
done

echo "✅ Context validation complete"

# Initialize execution log for this worktree
task_name=$(basename $(pwd))
cat > execution-log.md << 'EOF'
# Execution Log: [Task Name]
**Started**: $(date)
**Worktree**: $(pwd)
**Task ID**: TASK_ID_PLACEHOLDER
**Status**: IN_PROGRESS

## File Changes Tracking
### Estimated vs Actual Files
**Estimated Files** (from task file):
[Copy from task.md file scope section]

**Actual Files** (updated during execution):
[Update as files are modified]

### Unexpected Files
[Track any files not in original estimate]

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

## Context Application Log
[Track how context package was used during implementation]

## Integration Coordination
[Track any coordination with other parallel tasks]

## Completion Status
- [ ] All estimated files handled
- [ ] All build failures resolved
- [ ] All multi-fix files completed
- [ ] All deferred items documented
- [ ] Context properly applied
- [ ] Integration points verified
- [ ] Log reviewed and formatted
EOF

# Replace placeholder with actual task name
sed -i "s/TASK_ID_PLACEHOLDER/$task_name/" execution-log.md
sed -i "s/\\[Task Name\\]/$task_name/" execution-log.md

echo "✅ Execution log initialized"
```

### Context Application Process

#### 1. Read and Apply Context
```markdown
## Context Application Checklist
- [ ] **Read codebase-analysis.md**: Understand similar patterns and file dependencies
- [ ] **Read external-sources.md**: Review relevant documentation and standards
- [ ] **Read implementation-patterns.md**: Identify specific patterns to follow
- [ ] **Validate file scope**: Ensure all files in task scope exist and are accessible
```

#### 2. Implementation with Context Guidance & Logging
```markdown
## Context-Informed Implementation
### Before Writing Code:
1. **Find Similar Implementation**: Look for pattern from codebase-analysis.md
2. **Review External Standards**: Apply guidelines from external-sources.md  
3. **Follow Established Patterns**: Use patterns from implementation-patterns.md
4. **Validate Integration Points**: Ensure compatibility with other tasks

### During Implementation:
- **Reference Context**: Continuously refer to context package for guidance
- **Document Deviations**: Note any deviations from suggested patterns
- **Validate Frequently**: Run validation commands from task file
- **Update Context**: Document any new patterns discovered
- **Log Everything**: Use execution logging commands throughout

### Execution Logging Commands for Subagents
```bash
# Log web searches
log_web_search() {
    local search_terms="$1"
    local purpose="$2"
    local findings="$3"
    
    echo "
- **$(date)**: Searched for \"$search_terms\"
  - **Purpose**: $purpose
  - **Key findings**: $findings
  - **Applied**: [How it influenced implementation]" >> execution-log.md
}

# Log build failures
log_build_failure() {
    local error_message="$1"
    local command="$2"
    local fix="$3"
    
    echo "
- **$(date)**: Build failed with: $error_message
  - **Command**: \`$command\`
  - **Root cause**: [Why it failed]
  - **Fix applied**: $fix
  - **Resolution time**: [how long it took]" >> execution-log.md
}

# Track file changes
log_file_change() {
    local file_path="$1"
    local action="$2"
    
    # Update actual files section
    if grep -q "Actual Files" execution-log.md; then
        echo "- $file_path - [$action] ✅ $(date)" >> execution-log.md
    fi
}

# Log multi-fix files
log_multi_fix() {
    local file_path="$1"
    local fix_description="$2"
    
    echo "
- **$file_path**: Additional fix required
  - **Fix**: $fix_description
  - **Reason**: [Why additional fix was needed]" >> execution-log.md
}

# Log deferred items
log_deferred_item() {
    local item="$1"
    local reason="$2"
    
    echo "
- **$(date)**: $item
  - **Reason**: $reason
  - **Future task**: [When/how it should be addressed]
  - **Priority**: [High/Medium/Low]" >> execution-log.md
}

# Log new tasks
log_new_task() {
    local task="$1"
    local context="$2"
    
    echo "
- **$(date)**: $task
  - **Discovery context**: $context
  - **Estimated effort**: [Time/complexity]
  - **Priority**: [High/Medium/Low]" >> execution-log.md
}

# Log context application
log_context_usage() {
    local context_file="$1"
    local pattern_used="$2"
    
    echo "
- **$(date)**: Applied pattern from $context_file
  - **Pattern**: $pattern_used
  - **Implementation**: [How it was applied]" >> execution-log.md
}
```
```

### Task-Type Specific Execution

#### Research Tasks
```markdown
## Research Task Execution Pattern
1. **Context Review**: Read existing research in codebase-analysis.md
2. **Gap Analysis**: Identify what additional research is needed
3. **External Investigation**: Use external-sources.md as starting point
4. **Pattern Documentation**: Document findings for other tasks
5. **Context Updates**: Add new findings to context package

## Research Output Format:
- Update relevant context files with new findings
- Create research summary with actionable recommendations
- Document any patterns for development tasks to follow
```

#### Development Tasks  
```markdown
## Development Task Execution Pattern
1. **Pattern Selection**: Choose implementation pattern from context
2. **Similar Code Review**: Study similar implementation from codebase
3. **Standards Application**: Apply external standards from context
4. **Test-Driven Development**: Write tests first, following project patterns
5. **Context Validation**: Verify implementation matches context guidance

## Development Validation:
```bash
# Validate pattern application
grep -q "expected_pattern" modified_file.ts && echo "✅ Pattern applied"

# Validate external standards compliance  
npm run lint modified_file.ts && echo "✅ Standards compliant"

# Validate integration points
npm test -- --grep "integration" && echo "✅ Integration verified"

# Validate context consistency
diff <(grep "key_pattern" context/implementation-patterns.md) <(grep "key_pattern" modified_file.ts) && echo "✅ Context consistent"
```

#### Integration Tasks
```markdown
## Integration Task Execution Pattern  
1. **Cross-Task Analysis**: Review all completed development tasks
2. **Context Consistency**: Ensure all tasks followed same patterns
3. **Integration Testing**: Verify components work together
4. **Performance Validation**: Check integration meets context requirements
5. **Documentation Updates**: Update context with integration insights
```

### Error Handling & Context Issues

#### Context Problems & Critical Subagent Gotchas
```markdown
## Context Issue Resolution
### Missing Context Files:
1. **Contact Task Master**: Request context package regeneration
2. **Emergency Context**: Create minimal context from task file information
3. **Pattern Discovery**: Search codebase for similar implementations
4. **Documentation**: Note missing context for future improvement

### Outdated Context:
1. **Validation Check**: Verify external URLs still work
2. **Pattern Verification**: Confirm similar implementations still exist
3. **Update Request**: Request context package refresh from Task Master
4. **Local Updates**: Update context files with current information

### Context Conflicts:
1. **Pattern Conflicts**: When context suggests conflicting patterns
2. **Standard Conflicts**: When external standards conflict with codebase
3. **Escalation**: Report conflicts to Task Master for resolution
4. **Documentation**: Document resolution for future tasks

## CRITICAL Subagent Coordination Gotchas

### File Edit Coordination Issues
⚠️ **GOTCHA**: Multiple tasks editing same interface file
**Problem**: Race condition where Task A defines interface, Task B extends it simultaneously
**Solution**: 
```bash
# Check if file is being edited before starting
check_file_lock() {
    local file="$1"
    if [[ -f "$file.lock" ]]; then
        echo "❌ File locked by: $(cat $file.lock)"
        echo "Wait for completion or coordinate with task owner"
        return 1
    fi
    echo "$TASK_ID" > "$file.lock"
    return 0
}

# Always lock before editing shared files
check_file_lock "src/types/api.ts" || exit 1
```

⚠️ **GOTCHA**: Configuration file merge conflicts
**Problem**: Multiple tasks adding different config sections to same file
**Solution**: Use structured merge with validation
```bash
# Use structured config merging
merge_config_section() {
    local config_file="$1"
    local section_name="$2"
    local section_content="$3"
    
    # Add section marker for safe merging
    echo "# CONFIG_SECTION: $section_name - START" >> "$config_file"
    echo "$section_content" >> "$config_file"
    echo "# CONFIG_SECTION: $section_name - END" >> "$config_file"
}
```

### Context Synchronization Issues
⚠️ **GOTCHA**: Context becoming stale during long-running parallel development
**Problem**: External dependencies change, new patterns discovered, context package outdated
**Solution**: Context staleness detection
```bash
# Check context freshness
check_context_staleness() {
    local context_age=$(find ./context -name "*.md" -mtime +1 | wc -l)
    if [[ $context_age -gt 0 ]]; then
        echo "⚠️ Context may be stale (>24h old)"
        echo "Consider requesting context refresh from Task Master"
    fi
}
```

### Integration Timing Issues
⚠️ **GOTCHA**: Premature integration attempts
**Problem**: Task A completes, Task B at 50%, integration attempted too early
**Solution**: Integration readiness protocol
```bash
# Check integration readiness
check_integration_readiness() {
    echo "Checking integration readiness across all tasks..."
    local ready_count=0
    local total_count=0
    
    for task_dir in ~/work/worktrees/<feature>/*/; do
        total_count=$((total_count + 1))
        if [[ -f "$task_dir/TASK_COMPLETE" ]]; then
            ready_count=$((ready_count + 1))
            echo "✅ $(basename $task_dir): Ready"
        else
            echo "⏳ $(basename $task_dir): In progress"
        fi
    done
    
    if [[ $ready_count -eq $total_count ]]; then
        echo "✅ All tasks ready for integration"
        return 0
    else
        echo "⚠️ $((total_count - ready_count)) tasks still in progress"
        return 1
    fi
}
```

### Resource Contention Issues
⚠️ **GOTCHA**: Database/port conflicts during parallel testing
**Problem**: Multiple tasks trying to use same test database/ports
**Solution**: Resource allocation validation
```bash
# Validate resource allocation before starting
validate_resources() {
    local task_id="$1"
    
    # Check if allocated port is actually free
    if netstat -ln | grep ":$TASK_PORT "; then
        echo "❌ Port $TASK_PORT already in use"
        echo "Requesting new port allocation..."
        allocate_test_resources "$task_id"
    fi
    
    # Check if test database exists and is accessible
    psql "test_${task_id}" -c "SELECT 1;" 2>/dev/null || {
        echo "❌ Test database not accessible"
        echo "Recreating test database..."
        createdb "test_${task_id}"
    }
}
```

### Communication Overhead Issues
⚠️ **GOTCHA**: Over-communication vs under-communication
**Problem**: Subagents either spam Task Master or fail to communicate critical issues
**Solution**: Structured communication protocol
```bash
# Escalation decision tree
should_escalate() {
    local issue_type="$1"
    local impact="$2"
    
    case "$issue_type" in
        "merge_conflict")
            [[ "$impact" == "blocking" ]] && return 0 ;;
        "context_conflict") 
            return 0 ;;  # Always escalate context conflicts
        "resource_contention")
            [[ "$impact" == "blocking" ]] && return 0 ;;
        "integration_interface")
            return 0 ;;  # Always escalate interface changes
        *)
            return 1 ;;  # Don't escalate by default
    esac
    return 1
}

# Usage: should_escalate "merge_conflict" "blocking" && escalate_to_task_master
```
```

#### Implementation Blockers
```markdown
## Blocker Resolution with Context
### Technical Blockers:
1. **Context Review**: Check if similar problem solved in context
2. **Pattern Adaptation**: Adapt existing pattern to current need  
3. **External Research**: Use context sources for additional guidance
4. **Spike Task**: Create focused research task if needed

### Integration Blockers:
1. **Cross-Task Communication**: Coordinate with other subagents
2. **Context Alignment**: Ensure consistent pattern application
3. **Architecture Review**: Validate against architecture decisions
4. **Task Master Escalation**: Report integration conflicts
```

## Execution Monitoring & Coordination

### Real-Time Coordination
```bash
# Task Master monitoring script
monitor_tasks() {
    while true; do
        echo "=== Task Status Check $(date) ==="
        for task_dir in ~/work/worktrees/<feature>/*/; do
            task_name=$(basename "$task_dir")
            if [[ -f "$task_dir/status.txt" ]]; then
                status=$(cat "$task_dir/status.txt")
                echo "$task_name: $status"
            else
                echo "$task_name: Status unknown"
            fi
        done
        echo ""
        sleep 30
    done
}

# Subagent status reporting
report_status() {
    local status="$1"
    local message="$2"
    echo "$status: $message $(date)" > status.txt
    echo "Status updated: $status - $message"
}

# Usage in subagent:
# report_status "STARTED" "Context validated, beginning implementation"
# report_status "PROGRESS" "50% complete, patterns applied successfully"  
# report_status "COMPLETE" "All validation passed, ready for review"
# report_status "BLOCKED" "Missing dependency from task-02, waiting"
```

### Context Consistency Monitoring
```bash
# Validate context application across tasks
validate_context_consistency() {
    echo "Checking context consistency across all tasks..."
    
    for task_dir in ~/work/worktrees/<feature>/*/; do
        echo "Checking: $(basename "$task_dir")"
        
        # Check if context was accessed
        if [[ -f "$task_dir/.context_accessed" ]]; then
            echo "  ✅ Context accessed"
        else
            echo "  ⚠️ Context not accessed"
        fi
        
        # Check if patterns were followed
        if grep -q "pattern_applied" "$task_dir"/*.ts 2>/dev/null; then
            echo "  ✅ Patterns applied"
        else
            echo "  ⚠️ Pattern application not verified"
        fi
    done
}
```

## Execution Complete → Review Handoff

### Task Completion Requirements
```markdown
## Before Marking Task Complete:
- [ ] **Context Applied**: Implementation follows patterns from context
- [ ] **Standards Compliant**: External standards properly applied
- [ ] **Integration Ready**: Interfaces match architecture decisions
- [ ] **Tests Pass**: All validation commands successful
- [ ] **Documentation Updated**: Any new patterns added to context
- [ ] **Execution Log Complete**: All logging requirements fulfilled

## Execution Log Finalization:
```bash
# Complete execution log before marking task complete
complete_execution_log() {
    # Update completion status
    sed -i 's/- \[ \] All estimated files handled/- [x] All estimated files handled/' execution-log.md
    sed -i 's/- \[ \] All build failures resolved/- [x] All build failures resolved/' execution-log.md
    sed -i 's/- \[ \] All multi-fix files completed/- [x] All multi-fix files completed/' execution-log.md
    sed -i 's/- \[ \] All deferred items documented/- [x] All deferred items documented/' execution-log.md
    sed -i 's/- \[ \] Context properly applied/- [x] Context properly applied/' execution-log.md
    sed -i 's/- \[ \] Integration points verified/- [x] Integration points verified/' execution-log.md
    sed -i 's/- \[ \] Log reviewed and formatted/- [x] Log reviewed and formatted/' execution-log.md
    
    # Add final summary
    cat >> execution-log.md << EOF

## Final Summary
**Completed**: $(date)
**Status**: COMPLETE

### File Changes Summary
**Total Estimated Files**: $(grep -c "Estimated Files" execution-log.md || echo "0")
**Total Actual Files**: $(git diff --name-only HEAD~1 | wc -l)
**Variance**: [Calculate difference]

### Accuracy Assessment
- **Scope Accuracy**: [How accurate was the initial scope?]
- **Time Accuracy**: [Actual vs estimated time]
- **Context Effectiveness**: [How well did context guide implementation?]

### Integration Status
- **Cross-task Dependencies**: [Status of dependencies on other tasks]
- **Integration Points**: [How this integrates with other parallel work]
- **Coordination Notes**: [Any coordination issues or successes]

### Handoff Notes
- **Review Focus**: [Areas needing extra attention in review]
- **Risk Areas**: [Parts of implementation that need careful review]
- **Integration Testing**: [Specific integration tests needed]
EOF
}

# Run completion
complete_execution_log
```

## Completion Status Report:
```bash
# Create completion report
cat > completion_report.md << EOF
# Task Completion Report: $(basename $(pwd))

## Context Application Summary
- **Patterns Used**: [List patterns from context that were applied]
- **Standards Applied**: [List external standards implemented]
- **Similar Code Referenced**: [List similar implementations studied]

## Implementation Summary  
- **Files Modified**: [List actual files changed]
- **New Patterns Created**: [Any new patterns discovered]
- **Integration Points**: [How this connects to other tasks]

## Validation Results
- **Tests**: [All tests passing]
- **Linting**: [Code style compliant]
- **Integration**: [Integration tests passing]
- **Context Consistency**: [Implementation matches context guidance]

## MANDATORY: Branch Management Status
- **Feature Branch**: feature/${feature_name}-$(basename $(pwd))
- **Branch Status**: ✅ Created and pushed
- **Merge Status**: ✅ Worktree merged to feature branch
- **Workplan Status**: ✅ Committed to base branch
- **Execution Log**: ✅ Complete and committed

## Ready for Review Phase
- **Status**: COMPLETE
- **Next Phase**: [subtasks-review.md](subtasks-review.md)
- **Review Focus**: [Specific areas needing extra attention]
- **Integration Ready**: All branches created, workplan preserved, execution logged
EOF
```

### Handoff to Review Phase

## MANDATORY: Subtask Completion Results and Handoff

### Critical Completion Requirements

**⚠️ IMPORTANT**: When subtask execution completes, the Task Master MUST provide explicit verification of all results before handoff. Future agents will start with ZERO context from this execution session.

#### 1. Complete File Results Verification Across All Worktrees (REQUIRED)
```bash
# MANDATORY: Verify results from all parallel subtask worktrees
echo "=== SUBTASK EXECUTION RESULTS VERIFICATION ==="
echo "All worktree results and file modifications:"

for worktree in ~/work/worktrees/<feature>/*/; do
    task_name=$(basename "$worktree")
    echo "\n📋 Worktree: $task_name"
    echo "Location: $worktree"
    
    if [[ -f "$worktree/TASK_COMPLETE" ]]; then
        echo "Status: ✅ COMPLETE"
        
        # Show files modified in this worktree
        cd "$worktree"
        echo "Files modified:"
        git status --porcelain | while read status file; do
            echo "  $status $file"
        done
        
        # Show feature branch created
        current_branch=$(git branch --show-current)
        echo "Feature branch: $current_branch"
        echo "Commits: $(git rev-list --count HEAD ^main 2>/dev/null || echo 'N/A')"
    else
        echo "Status: ❌ INCOMPLETE"
    fi
done

echo "\n=== FEATURE BRANCHES CREATED ==="
git branch -r | grep "feature/<feature>-" | sed 's/.*\///g'
```

#### 2. Comprehensive Results Summary for User (REQUIRED)
```markdown
## SUBTASK EXECUTION COMPLETION SUMMARY

### All Worktree Results
**Worktree Base**: ~/work/worktrees/<feature>/

#### Task 1: [Name]
- **Location**: `/absolute/path/to/worktree1/`
- **Status**: ✅ Complete / ❌ Incomplete
- **Files Modified**: [List key files with absolute paths]
- **Feature Branch**: `feature/<feature>-task1`
- **Key Deliverables**: [What was implemented]

#### Task 2: [Name] 
- **Location**: `/absolute/path/to/worktree2/`
- **Status**: ✅ Complete / ❌ Incomplete
- **Files Modified**: [List key files with absolute paths]
- **Feature Branch**: `feature/<feature>-task2`
- **Key Deliverables**: [What was implemented]

[Continue for all tasks...]

### Integration Points Verified
- **Cross-Task Dependencies**: [Status of how tasks integrate]
- **Shared File Coordination**: [How shared files were handled]
- **Interface Compatibility**: [How task interfaces connect]

### Verification Commands for User
**User can verify all results with these commands:**
```bash
# Check all worktrees and their status
for worktree in ~/work/worktrees/<feature>/*/; do
    echo "Checking: $worktree"
    cd "$worktree" && git status && git log --oneline -3
done

# Check all feature branches created
git branch -r | grep "feature/<feature>-"

# Verify specific worktree results
cd ~/work/worktrees/<feature>/task1/
ls -la  # See files in worktree
cat key-file.ext  # Check specific implementation

# Verify workplan committed to base branch
ls -la docs/workplans/<feature>/
```

### Current State Summary
**What exists now across all tasks:**
- ✅ **Worktrees**: [Number] worktrees with completed implementations
- ✅ **Feature Branches**: [Number] feature branches pushed to origin
- ✅ **Workplan Archive**: Complete workplan with context preserved in docs/workplans/
- ✅ **Integration Points**: [Status of cross-task integration]
- ⏳ **Next Required**: Integration review and branch merging

### Next Steps Required
1. **Immediate**: Review all feature branches for integration compatibility
2. **Integration**: Merge compatible branches and resolve conflicts
3. **Testing**: Run cross-task integration tests
4. **Final Review**: Overall feature functionality validation
5. **Cleanup**: Remove worktrees after successful integration

### For Future Agents
**⚠️ CRITICAL**: Any future agent will need this context:
- **Feature Scope**: [Brief description of overall feature implemented]
- **Worktree Locations**: `~/work/worktrees/<feature>/` with [number] completed tasks
- **Feature Branches**: All branches prefixed with `feature/<feature>-`
- **Context Package**: Complete context preserved in `docs/workplans/<feature>/context/`
- **Integration Status**: [Current state of cross-task integration]
- **Outstanding Work**: [Any remaining integration or cleanup work]
```

#### 3. Branch Management State Documentation (REQUIRED)
```bash
# MANDATORY: Document complete branch state for handoff
echo "=== BRANCH MANAGEMENT STATE ==="
echo "Base branch: $(git branch --show-current)"
echo "Workplan committed: $(git log -1 --oneline --grep='workplan')"

echo "\nFeature branches created:"
for branch in $(git branch -r | grep "feature/<feature>-"); do
    branch_name=$(echo "$branch" | sed 's/.*\///g')
    echo "  $branch_name: $(git log -1 --oneline "$branch")"
done

echo "\n=== VERIFICATION COMMANDS ==="
echo "User can verify branch state with:"
echo "git branch -r | grep 'feature/<feature>-'"
echo "git log --oneline --graph --all | head -20"
echo "ls -la docs/workplans/<feature>/"
```

#### 4. Context and Integration Preservation (REQUIRED)
```markdown
## SUBTASK CONTEXT PRESERVATION CHECKLIST

### For User Reference
- [ ] **All Worktree Locations Documented**: Each worktree path and status listed
- [ ] **All Feature Branches Listed**: Every branch created with purpose
- [ ] **Verification Commands Provided**: User can independently verify all results
- [ ] **Integration Status Documented**: Clear description of cross-task integration state
- [ ] **Next Steps Defined**: Clear guidance on integration and review process
- [ ] **Workplan Archived**: Complete planning context preserved for future reference

### For Future Integration Sessions
- [ ] **Feature Scope Documented**: Overall feature purpose and boundaries
- [ ] **Task Breakdown Preserved**: How feature was divided into parallel tasks
- [ ] **Integration Points Identified**: How tasks were designed to work together
- [ ] **Context Package Available**: All planning context accessible in docs/workplans/
- [ ] **Branch Strategy Documented**: How branches should be merged and cleaned up
- [ ] **Testing Strategy Defined**: How to validate overall feature functionality

### Critical Files for Handoff
**Must be accessible for future sessions:**
- `docs/workplans/<feature>/context/` - Complete context package
- `docs/workplans/<feature>/completion.md` - Summary of what was accomplished
- Each worktree's execution-log.md - Detailed implementation notes
- Feature branches - All implementation code ready for integration
```

**All tasks complete** → **Next: [subtasks-review.md](subtasks-review.md)**

**Integration Verification**: All subagent outputs ready for cross-task integration review
**Context Validation**: Implementation consistency verified across all parallel tasks

## MANDATORY: Task Completion & Branch Management

### Critical Final Steps for Each Worktree

#### 1. Merge Worktree to Feature Branch (REQUIRED)
```bash
# MANDATORY: Every completed worktree must be merged into a feature branch
merge_worktree_to_branch() {
    local worktree_dir="$1"
    local task_name=$(basename "$worktree_dir")
    local feature_name="$2"
    
    cd "$worktree_dir"
    
    # Ensure all changes are committed
    if [[ -n $(git status --porcelain) ]]; then
        echo "❌ FATAL: Uncommitted changes in worktree"
        echo "Commit all changes before merging"
        exit 1
    fi
    
    # Create feature branch from worktree
    git checkout -b "feature/${feature_name}-${task_name}"
    
    # Push feature branch
    git push -u origin "feature/${feature_name}-${task_name}"
    
    echo "✅ Worktree merged to branch: feature/${feature_name}-${task_name}"
}

# Execute for each completed worktree
for worktree in ~/work/worktrees/<feature>/*/; do
    if [[ -f "$worktree/TASK_COMPLETE" ]]; then
        merge_worktree_to_branch "$worktree" "<feature>"
    fi
done
```

#### 2. Commit Workplan to Base Branch (REQUIRED)
```bash
# MANDATORY: Workplan must be committed to base branch for future reference
commit_workplan_to_base() {
    local feature_name="$1"
    local base_branch="main"  # or "master" or your default branch
    
    # Switch to base branch
    git checkout "$base_branch"
    git pull origin "$base_branch"
    
    # Ensure workplan directory exists
    mkdir -p "docs/workplans"
    
    # Copy complete workplan with all context
    cp -r "requests/${feature_name}/" "docs/workplans/${feature_name}/"
    
    # Add completion metadata
    cat > "docs/workplans/${feature_name}/completion.md" << EOF
# Workplan Completion Report

## Feature: ${feature_name}
- **Completion Date**: $(date)
- **Tasks Completed**: $(find ~/work/worktrees/${feature_name} -name "TASK_COMPLETE" | wc -l)
- **Feature Branches Created**: $(git branch -r | grep "feature/${feature_name}-" | wc -l)

## Implementation Summary
- **Context Package**: docs/workplans/${feature_name}/context/
- **Task Files**: docs/workplans/${feature_name}/tasks/
- **Integration Report**: docs/workplans/${feature_name}/integration/

## Next Steps
1. Review feature branches for integration
2. Create pull requests for each feature branch
3. Merge to main branch after review
4. Clean up worktrees: rm -rf ~/work/worktrees/${feature_name}
EOF

    # Commit workplan to base branch
    git add "docs/workplans/${feature_name}/"
    git commit -m "Add workplan documentation for ${feature_name}

- Complete task breakdown and context package
- All worktrees merged to feature branches
- Ready for integration review and merge

Feature branches:
$(git branch -r | grep "feature/${feature_name}-" | sed 's/.*\///g' | sed 's/^/- /')"

    git push origin "$base_branch"
    
    echo "✅ Workplan committed to base branch: $base_branch"
}
```

#### 3. Cleanup and Validation (REQUIRED)
```bash
# MANDATORY: Validate all worktrees are properly handled
validate_task_completion() {
    local feature_name="$1"
    local worktree_base="~/work/worktrees/${feature_name}"
    
    echo "=== Task Completion Validation ==="
    
    # Check all worktrees have been merged
    local unmerged_count=0
    for worktree in ${worktree_base}/*/; do
        task_name=$(basename "$worktree")
        if [[ -f "$worktree/TASK_COMPLETE" ]]; then
            # Check if feature branch exists
            if git branch -r | grep -q "feature/${feature_name}-${task_name}"; then
                echo "✅ Task $task_name: Merged to feature branch"
            else
                echo "❌ Task $task_name: NOT merged to feature branch"
                unmerged_count=$((unmerged_count + 1))
            fi
        else
            echo "⚠️ Task $task_name: Not marked complete"
        fi
    done
    
    # Check workplan committed to base
    if [[ -d "docs/workplans/${feature_name}" ]]; then
        echo "✅ Workplan committed to base branch"
    else
        echo "❌ Workplan NOT committed to base branch"
        unmerged_count=$((unmerged_count + 1))
    fi
    
    if [[ $unmerged_count -eq 0 ]]; then
        echo "✅ ALL TASKS PROPERLY COMPLETED AND MERGED"
        echo "Safe to proceed with integration review"
    else
        echo "❌ $unmerged_count tasks not properly handled"
        echo "MUST complete merge process before proceeding"
        exit 1
    fi
}
```

### Integration with Completion Workflow

#### Modified Completion Report
```bash
# Updated completion report with merge requirements
cat > completion_report.md << EOF
# Task Completion Report: $(basename $(pwd))

## Context Application Summary
- **Patterns Used**: [List patterns from context that were applied]
- **Standards Applied**: [List external standards implemented]
- **Similar Code Referenced**: [List similar implementations studied]

## Implementation Summary  
- **Files Modified**: [List actual files changed]
- **New Patterns Created**: [Any new patterns discovered]
- **Integration Points**: [How this connects to other tasks]

## Validation Results
- **Tests**: [All tests passing]
- **Linting**: [Code style compliant]
- **Integration**: [Integration tests passing]
- **Context Consistency**: [Implementation matches context guidance]

## MANDATORY: Branch Management Status
- **Feature Branch**: feature/${feature_name}-$(basename $(pwd))
- **Branch Status**: ✅ Created and pushed
- **Merge Status**: ✅ Worktree merged to feature branch
- **Workplan Status**: ✅ Committed to base branch

## Ready for Review Phase
- **Status**: COMPLETE
- **Next Phase**: [subtasks-review.md](subtasks-review.md)
- **Review Focus**: [Specific areas needing extra attention]
- **Integration Ready**: All branches created, workplan preserved
EOF
```

### Final Execution Checklist

```markdown
## Task Master Final Checklist (MANDATORY)

### Before Handoff to Review:
- [ ] **All Tasks Complete**: Every worktree has TASK_COMPLETE file
- [ ] **Feature Branches Created**: Each worktree merged to feature/name-task branch
- [ ] **Branches Pushed**: All feature branches pushed to origin
- [ ] **Workplan Committed**: Complete workplan committed to base branch
- [ ] **Validation Passed**: validate_task_completion() returns success
- [ ] **Worktree Cleanup**: All worktrees ready for cleanup after review

### Handoff Command:
```bash
# Execute final validation before handoff
validate_task_completion "<feature_name>"

# If validation passes, proceed to review
echo "✅ All tasks properly completed and merged"
echo "Proceeding to subtasks-review.md"
```

**⚠️ CRITICAL**: Do NOT proceed to review phase until:
1. All worktrees are merged to feature branches
2. Workplan is committed to base branch  
3. User has been provided with complete verification of all results
4. Future agent context has been documented
5. All file locations and verification commands have been provided
```