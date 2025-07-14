# Subtasks Review & Integration Verification
## Cross-Task Integration Review for Large Features

## Quick Review Decision Tree

### Choose Review Depth:
- **🚀 Simple Integration** (2-3 tasks, low complexity) → Quick Integration Check below
- **⚡ Standard Feature** (4-8 tasks, moderate complexity) → Standard Integration Review
- **🎯 Complex Feature** (>8 tasks, high complexity, multiple domains) → Full Integration Methodology

---

## Quick Integration Check (Simple Features)

### Essential Cross-Task Validation:
```bash
# Run integration validation across all worktrees
cd ~/work/worktrees/<feature>/

# Context consistency check
for task_dir in */; do
    echo "Checking context application: $task_dir"
    if [[ -f "$task_dir/completion_report.md" ]]; then
        grep -q "Context Applied" "$task_dir/completion_report.md" && echo "✅ Context applied"
    else
        echo "❌ Missing completion report"
    fi
done

# Integration tests
npm run test:integration -- --grep "<feature>"
npm run lint
npm run typecheck
```

### Quick Integration Checklist:
- [ ] All subagent tasks completed with context applied
- [ ] Integration tests pass across all components
- [ ] No conflicts between parallel implementations
- [ ] Feature flags implemented (if replacing functionality)
- [ ] Context consistency maintained across tasks

**Simple features**: If all pass → Go to [subtasks-complete.md](subtasks-complete.md) (Quick Completion)

---

## Standard Integration Review

### 1. Context Application Verification

#### Cross-Task Context Consistency
```markdown
## Context Application Review

### Pattern Consistency Analysis
- [ ] **Architecture Patterns**: All tasks followed same architectural decisions
- [ ] **Code Patterns**: Similar implementations referenced consistently
- [ ] **Naming Conventions**: Consistent naming across all components
- [ ] **Error Handling**: Uniform error handling patterns applied
- [ ] **Testing Patterns**: Consistent test structure and coverage

### External Standards Compliance
- [ ] **Documentation Standards**: All tasks referenced same external docs
- [ ] **Security Standards**: Security guidelines consistently applied
- [ ] **Performance Standards**: Performance targets met across tasks
- [ ] **Accessibility**: Accessibility guidelines uniformly implemented
```

#### Context Source Verification
```bash
# Verify all tasks used consistent context sources
echo "Verifying context source consistency..."

for task_dir in ~/work/worktrees/<feature>/*/; do
    task_name=$(basename "$task_dir")
    echo "Checking: $task_name"
    
    # Check if context was properly accessed
    if [[ -f "$task_dir/completion_report.md" ]]; then
        echo "  Patterns used:"
        grep "Patterns Used:" "$task_dir/completion_report.md" | sed 's/.*: /    - /'
        echo "  Standards applied:"
        grep "Standards Applied:" "$task_dir/completion_report.md" | sed 's/.*: /    - /'
    else
        echo "  ❌ No completion report found"
    fi
    echo ""
done
```

### 2. Integration Points Verification

#### API Contract Validation
```markdown
## API Integration Review

### Service Interface Consistency
- [ ] **Request/Response Schemas**: All APIs use consistent data structures
- [ ] **Error Response Format**: Uniform error handling across services
- [ ] **Authentication**: Consistent auth implementation across endpoints
- [ ] **Versioning**: API versioning strategy consistently applied
- [ ] **Documentation**: API documentation updated and consistent

### Database Integration
- [ ] **Schema Consistency**: Database changes compatible across tasks
- [ ] **Migration Compatibility**: All migrations can run in sequence
- [ ] **Data Integrity**: Foreign key relationships properly maintained
- [ ] **Performance Impact**: Database changes don't create bottlenecks
```

#### Component Integration Testing
```bash
# Integration test execution across all components
echo "Running comprehensive integration tests..."

# Test component interactions
npm run test:integration -- --grep "<feature>" --verbose

# Test API integration
npm run test:api -- --grep "<feature>"

# Test database integration  
npm run test:db -- --grep "<feature>"

# Test end-to-end workflows
npm run test:e2e -- --grep "<feature>"

# Performance regression test
npm run test:performance -- --baseline
```

### 3. Cross-Task Dependency Resolution

#### Dependency Mapping Verification
```markdown
## Task Dependency Analysis

### Completed Task Dependencies
- **Task A** → **Task B**: [Interface verified] ✅
- **Task B** → **Task C**: [Data flow tested] ✅  
- **Task C** → **Task D**: [Integration working] ❌ (Issue found)

### Integration Issues Found

#### Critical Subagent Coordination Issues
- **File Merge Conflicts**: [Specific files with conflicts]
  - **Root Cause**: [Multiple tasks editing same files without coordination]
  - **Resolution**: [Use file locking protocol, interface versioning]
  - **Tasks Affected**: [List conflicting tasks]

- **Context Application Inconsistency**: [Pattern mismatches across tasks]
  - **Root Cause**: [Context package staleness or misinterpretation]
  - **Resolution**: [Context refresh, pattern clarification]
  - **Tasks Affected**: [Tasks with inconsistent patterns]

- **Resource Contention**: [Database/port conflicts]
  - **Root Cause**: [Inadequate resource allocation]
  - **Resolution**: [Implement resource isolation per task]
  - **Tasks Affected**: [Tasks with resource conflicts]

#### Standard Integration Issues
- **Issue**: [Specific integration problem]
- **Root Cause**: [Why dependency failed]
- **Resolution**: [How to fix] 
- **Tasks Affected**: [Which tasks need updates]

### Circular Dependency Check
- [ ] **No Circular Dependencies**: All task dependencies form valid DAG
- [ ] **Clean Interfaces**: Components interact through well-defined interfaces
- [ ] **Loose Coupling**: Changes in one task don't break others unnecessarily
```

#### Resolution Process
```bash
# Enhanced conflict detection for parallel development
echo "Scanning for integration conflicts..."

# Check for file merge conflicts (critical for parallel development)
check_merge_conflicts() {
    echo "=== Checking for merge conflicts ==="
    for task_dir in ~/work/worktrees/<feature>/*/; do
        task_name=$(basename "$task_dir")
        echo "Checking: $task_name"
        
        # Check for git merge conflicts
        git -C "$task_dir" status | grep -q "both modified\|both added" && \
            echo "  ❌ MERGE CONFLICT: $task_name has unresolved conflicts"
            
        # Check for lock files (indicates coordination needed)
        find "$task_dir" -name "*.lock" | while read lock_file; do
            echo "  ⚠️ LOCK FILE: $(basename $lock_file) - coordination needed"
        done
        
        # CRITICAL: Check for interface version conflicts
        find "$task_dir" -name "*.types.ts" -o -name "*.interface.ts" | while read interface_file; do
            base_name=$(basename "$interface_file")
            # Check if other tasks have modified the same base interface
            for other_dir in ~/work/worktrees/<feature>/*/; do
                [[ "$other_dir" == "$task_dir" ]] && continue
                if [[ -f "$other_dir/$base_name" ]]; then
                    echo "  ⚠️ INTERFACE CONFLICT: $base_name modified by multiple tasks"
                    echo "    Tasks: $task_name, $(basename $other_dir)"
                fi
            done
        done
        
        # Check for configuration merge issues
        find "$task_dir" -name "*.config.*" -o -name "package.json" | while read config_file; do
            if git -C "$task_dir" diff --name-only | grep -q "$(basename $config_file)"; then
                echo "  ⚠️ CONFIG CHANGE: $(basename $config_file) modified - check for conflicts"
            fi
        done
    done
}

# Check for context application inconsistencies
check_context_consistency() {
    echo "=== Checking context application consistency ==="
    
    # Compare pattern usage across tasks
    declare -A pattern_usage
    for task_dir in ~/work/worktrees/<feature>/*/; do
        task_name=$(basename "$task_dir")
        if [[ -f "$task_dir/completion_report.md" ]]; then
            patterns=$(grep "Patterns Used:" "$task_dir/completion_report.md" | cut -d: -f2)
            pattern_usage["$task_name"]="$patterns"
        fi
    done
    
    # Check for pattern inconsistencies
    for task in "${!pattern_usage[@]}"; do
        echo "Task $task uses patterns: ${pattern_usage[$task]}"
    done
}

# Check for resource contention
check_resource_contention() {
    echo "=== Checking resource contention ==="
    
    # Check for port conflicts
    netstat -ln | grep ":300[0-9]" | while read line; do
        port=$(echo "$line" | grep -o ":300[0-9]")
        echo "⚠️ Port conflict detected: $port"
    done
    
    # Check for database conflicts
    psql -l | grep "test_" | while read db_line; do
        db_name=$(echo "$db_line" | awk '{print $1}')
        echo "Test database in use: $db_name"
    done
}

# Run all enhanced conflict checks
check_merge_conflicts
check_context_consistency  
check_resource_contention

# Original conflict checks
# Check for naming conflicts
find ~/work/worktrees/<feature>/ -name "*.ts" -exec grep -l "export.*function\|export.*class" {} \; | \
    xargs grep "export" | sort | uniq -d | \
    grep "duplicate" && echo "⚠️ Naming conflicts found"

# Check for type conflicts
npm run typecheck 2>&1 | grep -i conflict && echo "⚠️ Type conflicts found"

# Check for dependency conflicts
npm ls --depth=0 | grep -i "UNMET\|ERR" && echo "⚠️ Dependency conflicts found"
```

### 4. Feature Flag & Replacement Verification

#### Feature Flag Implementation Review
```markdown
## Feature Flag Validation (for replacements)

### Flag Implementation Consistency
- [ ] **Flag Definition**: Feature flag defined consistently across all tasks
- [ ] **Default State**: Flag defaults to safe state (old functionality)
- [ ] **Flag Checking**: All new code properly checks flag state
- [ ] **Graceful Degradation**: System works with flag disabled
- [ ] **Toggle Safety**: Flag can be toggled without deployment

### Parallel Operation Verification
- [ ] **Both Versions Work**: Old and new implementations both functional
- [ ] **Data Compatibility**: Both versions work with same data
- [ ] **Performance Comparison**: New version performance measured vs old
- [ ] **User Experience**: UX consistent between versions
- [ ] **Monitoring**: Both versions properly monitored
```

#### Replacement Safety Validation
```bash
# Test both old and new implementations
echo "Testing feature flag safety..."

# Test with flag disabled (old functionality)
FEATURE_FLAG=false npm test -- --grep "<feature>"
FEATURE_FLAG=false npm run test:integration -- --grep "<feature>"

# Test with flag enabled (new functionality)  
FEATURE_FLAG=true npm test -- --grep "<feature>"
FEATURE_FLAG=true npm run test:integration -- --grep "<feature>"

# Test flag toggle safety
echo "Testing flag toggle scenarios..."
# Run tests while toggling flag to ensure no crashes
```

## Full Integration Methodology (Complex Features)

### 1. Comprehensive Context Audit

#### Context Application Audit
```markdown
## Master Context Application Audit

### Context Package Verification
- [ ] **All Context Files Used**: Every context file referenced by at least one task
- [ ] **Consistent Application**: Same context applied consistently across tasks
- [ ] **No Context Gaps**: No tasks implemented without proper context
- [ ] **Context Updates**: Context package updated with new discoveries

### Pattern Implementation Audit
For each pattern in context/implementation-patterns.md:
- [ ] **Pattern A**: Used by [list tasks] - Consistently implemented ✅
- [ ] **Pattern B**: Used by [list tasks] - Needs standardization ⚠️
- [ ] **Pattern C**: Unused - Consider removing or documenting why

### External Standards Compliance Audit
For each standard in context/external-sources.md:
- [ ] **Standard A**: [Compliance level] - [Evidence]
- [ ] **Standard B**: [Compliance level] - [Evidence]
- [ ] **Standard C**: [Compliance level] - [Evidence]
```

### 2. Architecture Integrity Verification

#### System Architecture Review
```markdown
## Architecture Decision Implementation Review

### Architecture Compliance
- [ ] **Service Boundaries**: All tasks respect defined service boundaries
- [ ] **Data Flow**: Data flows according to architectural decisions
- [ ] **Integration Patterns**: Consistent integration patterns used
- [ ] **Scalability**: Implementation supports scalability requirements
- [ ] **Security**: Security architecture properly implemented

### Performance Architecture Review
- [ ] **Performance Targets**: All components meet performance requirements
- [ ] **Bottleneck Analysis**: No new performance bottlenecks introduced
- [ ] **Resource Usage**: Resource consumption within acceptable limits
- [ ] **Caching Strategy**: Caching implemented according to architecture
- [ ] **Database Performance**: Database access patterns optimized
```

### 3. Quality Assurance Review

#### Code Quality Across Tasks
```bash
# Comprehensive quality analysis
echo "Running comprehensive quality checks..."

# Code complexity analysis
npm run complexity -- src/ | grep -E "high|complex" && echo "⚠️ High complexity found"

# Security analysis
npm audit --audit-level high
npm run security-scan

# Performance analysis
npm run benchmark -- --compare-baseline

# Test coverage analysis
npm run coverage -- --threshold 80
```

#### Documentation & Knowledge Transfer
```markdown
## Documentation Review

### Context Documentation Quality
- [ ] **Codebase Analysis**: Complete and accurate
- [ ] **External Sources**: All URLs working and relevant
- [ ] **Implementation Patterns**: Clear and usable
- [ ] **Architecture Decisions**: Well documented with rationale

### Feature Documentation
- [ ] **API Documentation**: Complete and accurate
- [ ] **User Documentation**: Clear and comprehensive
- [ ] **Developer Documentation**: Setup and development guide
- [ ] **Deployment Documentation**: Deployment and rollback procedures
```

## Integration Issues & Resolution

### Issue Classification & Resolution

#### Critical Integration Issues
```markdown
## CRITICAL: Must Fix Before Completion
- **Data Loss Risk**: [Issue description] - [Resolution plan]
- **Security Vulnerability**: [Issue description] - [Resolution plan]
- **System Instability**: [Issue description] - [Resolution plan]
- **Breaking Changes**: [Issue description] - [Resolution plan]

## HIGH PRIORITY: Should Fix Before Completion
- **Performance Degradation**: [Issue description] - [Resolution plan]
- **UX Inconsistency**: [Issue description] - [Resolution plan]
- **Integration Complexity**: [Issue description] - [Resolution plan]

## MEDIUM PRIORITY: Address in Follow-up
- **Code Quality**: [Issue description] - [Future task]
- **Documentation Gaps**: [Issue description] - [Future task]
- **Optimization Opportunities**: [Issue description] - [Future task]
```

#### Issue Resolution Process
```bash
# Create integration fix tasks
create_fix_task() {
    local issue="$1"
    local priority="$2"
    local task_id="fix-$(date +%s)"
    
    echo "Creating fix task: $task_id for $issue"
    
    # Create worktree for fix
    git worktree add "~/work/worktrees/<feature>/$task_id" feature/<feature>
    
    # Create fix task file with context
    cat > "~/work/worktrees/<feature>/$task_id/task.md" << EOF
# Fix Task: $issue
Priority: $priority
Context: Available in ./context/
Integration Issue: $issue
Resolution Plan: [Specific steps]
EOF
}
```

## Review Complete → Completion Handoff

### Integration Review Report
```markdown
# Integration Review Report: <Feature Name>

## Review Summary
**Date**: [Date]
**Reviewer**: [Name/Role]
**Tasks Reviewed**: [List of all tasks]
**Integration Status**: ✅ Approved / ⚠️ Approved with Conditions / ❌ Rejected

## Context Application Assessment
**Score**: [X/5]
- **Pattern Consistency**: [Assessment]
- **Standards Compliance**: [Assessment] 
- **Context Usage**: [Assessment]

## Integration Quality Assessment  
**Score**: [X/5]
- **Component Integration**: [Assessment]
- **API Consistency**: [Assessment]
- **Database Integration**: [Assessment]
- **Performance Impact**: [Assessment]

## Issues Found & Resolution Status
### Critical Issues: [X]
- [Issue 1]: [Status]
- [Issue 2]: [Status]

### High Priority Issues: [X]
- [Issue 1]: [Status]
- [Issue 2]: [Status]

## Recommendations
- **Architecture**: [Suggestions for architecture improvements]
- **Context Process**: [Improvements for context application]
- **Integration Process**: [Improvements for future integration]

## Sign-off
- [ ] All critical issues resolved
- [ ] Integration tests passing
- [ ] Context consistency verified
- [ ] Feature ready for completion phase

**Review Complete** → **Next: [subtasks-complete.md](subtasks-complete.md)**
```