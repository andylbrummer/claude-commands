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

### Context Validation (Required First Step)
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

#### 2. Implementation with Context Guidance
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

## Ready for Review Phase
- **Status**: COMPLETE
- **Next Phase**: [subtasks-review.md](subtasks-review.md)
- **Review Focus**: [Specific areas needing extra attention]
EOF
```

### Handoff to Review Phase
**All tasks complete** → **Next: [subtasks-review.md](subtasks-review.md)**

**Integration Verification**: All subagent outputs ready for cross-task integration review
**Context Validation**: Implementation consistency verified across all parallel tasks