# Subtasks Prototype-First Execution Guidelines
## Prototype → Production Workflow with Context Preservation

## Prerequisites Check

### MANDATORY: Before Executing Prototype Tasks
```markdown
## Prototype Planning Verification ✅
- [ ] **Master Planning Complete**: Large feature planning completed using [subtasks-plan.md](subtasks-plan.md)
- [ ] **Context Package Created**: Comprehensive context package exists in `/requests/<feature>/context/`
- [ ] **Initial Task Files Generated**: Task files created based on current assumptions
- [ ] **Prototype Worktree Structure**: Git worktrees set up in `~/work/worktrees/<feature>/proto/`
- [ ] **Production Planning Reserved**: Production planning deferred until prototype insights available

## Assumption Testing Framework
- [ ] **Critical Assumptions Identified**: List of key assumptions that could invalidate current plan
- [ ] **Risk Areas Mapped**: High-uncertainty areas requiring validation
- [ ] **Alternative Approaches Listed**: Multiple approaches to test for each risk area
- [ ] **Plan Adjustment Process**: Clear process for updating plan based on prototype findings
- [ ] **Failure Criteria Defined**: When to abandon approach and try alternative

## Prototype Validation Targets
- [ ] **External Dependencies**: API capabilities, data availability, service limits
- [ ] **Technical Feasibility**: Performance characteristics, integration complexity
- [ ] **Architecture Assumptions**: Component interaction patterns, data flow
- [ ] **Implementation Complexity**: Time estimates, skill requirements, tooling needs
- [ ] **User Experience Viability**: Interface concepts, workflow feasibility

## Plan Flexibility Preparation
- [ ] **Task Decomposition Flexibility**: Prepared to split/merge tasks based on findings
- [ ] **Dependency Re-mapping**: Ready to adjust task dependencies based on prototype results
- [ ] **Timeline Elasticity**: Buffer time allocated for plan adjustments
- [ ] **Resource Reallocation**: Prepared to shift resources based on complexity discoveries

**⚠️ STOP**: Do not proceed until all critical assumptions are identified and multiple approaches are prepared for testing
```

## Quick Start: Prototype-First Execution

### For Task Master (Orchestrator):
1. ✅ Context package created from [subtasks-plan.md](subtasks-plan.md)
2. ✅ Critical assumptions documented and alternative approaches prepared
3. ✅ Initial prototype worktrees set up in `~/work/worktrees/<feature>/proto/`
4. Start assumption testing process below
5. ✅ Plan adjustment process ready for prototype findings

### For Assumption Testing Subagents:
1. ✅ Receive assumption testing objectives with context package
2. ✅ Build rapid prototypes to validate/invalidate specific assumptions
3. Test multiple approaches when assumptions prove false
4. → Report findings that impact overall plan and task structure

### For Plan Adjustment Phase:
1. ✅ Aggregate all assumption testing results
2. ✅ Revise overall plan based on prototype discoveries
3. ✅ Re-generate task files with updated approach and dependencies
4. → Create production implementation plan based on validated approach

### For Production Subagents:
1. ✅ Receive validated approach and updated context from prototype phase
2. ✅ Follow proven patterns and avoid prototype-identified pitfalls
3. Implement with confidence in validated technical approach
4. → Report final implementation completion

---

## Assumption Testing Coordination Process

### Task Master Responsibilities

#### 1. Critical Assumption Documentation (MANDATORY FIRST STEP)

##### Assumption Registry Setup
```bash
# Create assumption testing framework
setup_assumption_testing() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    
    echo "=== Setting up assumption testing framework ==="
    
    # Create assumption registry
    mkdir -p "requests/${feature_name}/assumptions"
    
    cat > "requests/${feature_name}/assumptions/critical-assumptions.md" << EOF
# Critical Assumptions Registry

## External Dependency Assumptions
### API Assumptions
- **Assumption**: API X provides field Y with expected format Z
- **Risk Level**: HIGH (could invalidate multiple tasks)
- **Test Method**: Direct API testing prototype
- **Failure Impact**: Need alternative data source or restructure data layer
- **Alternative Approaches**: [List backup approaches]

### Data Availability Assumptions
- **Assumption**: Required data is available in expected format
- **Risk Level**: HIGH
- **Test Method**: Data exploration prototype
- **Failure Impact**: Task scope changes, new data processing tasks
- **Alternative Approaches**: [List data transformation strategies]

### Service Integration Assumptions
- **Assumption**: Service X supports expected integration pattern
- **Risk Level**: MEDIUM
- **Test Method**: Integration spike prototype
- **Failure Impact**: Architecture changes needed
- **Alternative Approaches**: [List integration alternatives]

## Technical Feasibility Assumptions
### Performance Assumptions
- **Assumption**: Approach X will meet performance requirement Y
- **Risk Level**: HIGH
- **Test Method**: Performance prototype with realistic data volume
- **Failure Impact**: Need performance optimization or different approach
- **Alternative Approaches**: [List performance strategies]

### Complexity Assumptions
- **Assumption**: Implementation X is feasible within time constraint Y
- **Risk Level**: MEDIUM
- **Test Method**: Time-boxed implementation spike
- **Failure Impact**: Task decomposition or timeline adjustment
- **Alternative Approaches**: [List simpler approaches]

## Architecture Assumptions
### Component Integration Assumptions
- **Assumption**: Components will integrate as designed
- **Risk Level**: HIGH
- **Test Method**: End-to-end integration prototype
- **Failure Impact**: Architecture redesign, task dependency changes
- **Alternative Approaches**: [List integration patterns]

## TEMPLATE for New Assumptions:
- **Assumption**: [Clear statement of what we believe]
- **Risk Level**: [HIGH/MEDIUM/LOW - impact if wrong]
- **Test Method**: [How to validate quickly]
- **Failure Impact**: [What changes if assumption is false]
- **Alternative Approaches**: [Backup plans ready to test]
EOF

    echo "✅ Assumption registry created"
}

# Create assumption-specific prototype worktrees
create_assumption_prototypes() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    
    echo "=== Creating assumption testing prototypes ==="
    
    # Create prototype for each major assumption cluster
    # These are NOT task-based, they're assumption-based
    
    # API/External dependency testing
    git worktree add "${proto_base}/api-validation" -b "proto/${feature_name}-api-validation"
    create_assumption_prototype "${proto_base}/api-validation" "API_VALIDATION" "
## Assumption Testing Goals:
- Validate API data availability and format
- Test service integration capabilities
- Identify data transformation requirements
- Discover API limitations and alternatives

## Testing Approach:
- Build minimal API integration
- Test with realistic data volumes
- Document actual vs expected API behavior
- Test multiple endpoints/services
"

    # Technical feasibility testing
    git worktree add "${proto_base}/tech-feasibility" -b "proto/${feature_name}-tech-feasibility"
    create_assumption_prototype "${proto_base}/tech-feasibility" "TECH_FEASIBILITY" "
## Assumption Testing Goals:
- Validate performance characteristics
- Test implementation complexity
- Identify technical blockers
- Test alternative technical approaches

## Testing Approach:
- Build core functionality with realistic constraints
- Performance test with expected data volumes
- Try multiple implementation approaches
- Document complexity vs estimates
"

    # Architecture integration testing
    git worktree add "${proto_base}/arch-integration" -b "proto/${feature_name}-arch-integration"
    create_assumption_prototype "${proto_base}/arch-integration" "ARCH_INTEGRATION" "
## Assumption Testing Goals:
- Validate component integration patterns
- Test data flow assumptions
- Identify integration complexity
- Test alternative architecture patterns

## Testing Approach:
- Build simplified end-to-end flow
- Test component communication
- Validate data transformation requirements
- Test integration failure scenarios
"

    echo "✅ Assumption testing prototypes created"
}

# Helper function to create assumption prototype environment
create_assumption_prototype() {
    local proto_dir="$1"
    local test_type="$2"
    local goals="$3"
    
    # Link context package
    ln -sf "$(pwd)/requests/${feature_name}/context" "${proto_dir}/context"
    
    # Create assumption testing environment
    cat > "${proto_dir}/ASSUMPTION_TESTING_MODE" << EOF
# ASSUMPTION TESTING ENVIRONMENT
This prototype exists to validate critical assumptions.

## Primary Purpose: ASSUMPTION VALIDATION
- Test specific assumptions that could invalidate current plan
- Build multiple approaches when assumptions prove false
- Focus on learning, not implementation quality
- Document findings that require plan adjustments

$goals

## Testing Methodology:
1. Start with highest-risk assumption
2. Build minimal test to validate/invalidate
3. If assumption fails, immediately test alternative approach
4. Document impact on overall plan and task structure
5. Test multiple alternatives until viable approach found

## Plan Impact Tracking:
- Which tasks need scope changes?
- Which dependencies need adjustment?  
- What new tasks are needed?
- What assumptions were wrong and why?

## Success Criteria:
- All critical assumptions validated or alternatives found
- Clear understanding of implementation complexity
- Plan adjustment recommendations documented
- Validated approach ready for production implementation
EOF

    echo "✅ Assumption testing environment created: $proto_dir"
}
```

##### Production Worktree Preparation
```bash
# Prepare production worktrees (don't create yet)
prepare_production_worktrees() {
    local feature_name="$1"
    local final_base="~/work/worktrees/${feature_name}/final"
    
    echo "=== Preparing production worktree plan ==="
    
    # Create production planning directory
    mkdir -p "requests/${feature_name}/production-plan"
    
    cat > "requests/${feature_name}/production-plan/worktree-plan.md" << EOF
# Production Worktree Plan

## Prototype Phase Complete
Once all prototypes are complete and lessons learned, create production worktrees:

\`\`\`bash
# Create production worktrees on feature/* branches  
for task_name in \$(ls ~/work/worktrees/${feature_name}/proto/); do
    git worktree add "${final_base}/\${task_name}" -b "feature/${feature_name}-\${task_name}"
    
    # Copy lessons learned and updated context
    cp -r "~/work/worktrees/${feature_name}/proto/\${task_name}/LESSONS_LEARNED.md" "${final_base}/\${task_name}/"
    ln -sf "\$(pwd)/requests/${feature_name}/context" "${final_base}/\${task_name}/context"
done
\`\`\`

## Context Updates from Prototype
- Update implementation-patterns.md with prototype findings
- Add performance-requirements.md from prototype testing
- Create integration-lessons.md from prototype integration testing
- Update risk-mitigation.md with prototype-validated solutions

## Production Task Updates  
- Refine task scope based on prototype complexity discovery
- Update time estimates based on prototype development speed
- Adjust integration points based on prototype interface testing
- Incorporate lessons learned into production task requirements
EOF

    echo "✅ Production worktree plan documented"
}
```

#### 2. Assumption Testing Execution

##### Assumption Testing Priority
```markdown
## Assumption Testing Priority Matrix
| Risk Level | AI Testing Effort | Priority | Action | Max Time |
|------------|-------------------|----------|---------|----------|
| HIGH | LOW | P0 | Test immediately | 2-4 hours |
| HIGH | MEDIUM | P1 | Test with time limit | 4-8 hours |
| HIGH | HIGH | P2 | Test core only, document risk | 8 hours max |
| MEDIUM | LOW | P3 | Test if P0-P2 complete | 2 hours |
| MEDIUM | MEDIUM/HIGH | P4 | Document risk, monitor | No testing |
| LOW | ANY | P5 | Document, accept risk | No testing |

## Agentic AI Testing Strategy
**Core Hypothesis**: AI coding speed + multiple prototypes = higher quality than single implementation

### Speed Advantage Exploitation
- **Rapid Iteration**: AI can build 3-5 prototype approaches in time for 1 careful implementation
- **Parallel Testing**: Test multiple assumptions simultaneously with AI agents
- **Fast Failure Discovery**: Quickly identify failed assumptions before investing in full implementation
- **Quality Through Comparison**: Best approach emerges from testing alternatives

## Assumption Testing Assignment Process
1. **P0 High-Risk/Low-Effort**: Test immediately with AI agents (APIs, data format validation)
2. **P1 High-Risk/Medium-Effort**: Time-boxed testing with multiple AI approaches
3. **P2 High-Risk/High-Effort**: Core validation only, use AI speed to test quickly
4. **External Dependencies First**: AI can quickly test API integrations and data availability
5. **Multiple AI Approaches**: When assumptions fail, spawn 2-3 AI agents with different solutions

## Assumption Testing Subagent Spawn Commands
```bash
# Start with highest-risk external dependencies (sequential - must complete first)
claude-code --worktree ~/work/worktrees/<feature>/proto/api-validation/ "ASSUMPTION TESTING: Validate API data availability and integration patterns. Test multiple approaches if assumptions prove false."

# Wait for API validation results before proceeding
wait_for_assumption_results "api-validation"

# Based on API results, may need to spawn additional alternative testing
if api_assumptions_failed; then
    claude-code --worktree ~/work/worktrees/<feature>/proto/api-alternative-1/ "ASSUMPTION TESTING: Test alternative data source approach based on API validation failures"
    claude-code --worktree ~/work/worktrees/<feature>/proto/api-alternative-2/ "ASSUMPTION TESTING: Test data transformation approach for API limitations"
fi

# Parallel technical feasibility testing (can run together)
claude-code --worktree ~/work/worktrees/<feature>/proto/tech-feasibility/ "ASSUMPTION TESTING: Validate performance and complexity assumptions with realistic constraints" &
claude-code --worktree ~/work/worktrees/<feature>/proto/arch-integration/ "ASSUMPTION TESTING: Test component integration patterns and data flow" &

# Wait for all assumption testing before plan adjustment
wait
```

##### Assumption Testing Results Monitoring
```markdown
## Assumption Validation Status Tracking
| Assumption Category | Test Status | Result | Plan Impact | Alternative Tested |
|-------------------|-------------|--------|-------------|-------------------|
| API Data Format | ✅ Complete | ❌ FAILED | HIGH - Need data transformation tasks | ✅ Transformation approach validated |
| Performance Requirements | 🔄 Testing | ⏳ In Progress | TBD | Multiple approaches prepared |
| Service Integration | ✅ Complete | ✅ VALIDATED | NONE | N/A |
| Implementation Complexity | ⏸️ Blocked | ⏳ Waiting | TBD | Depends on API results |

## Plan Adjustment Tracking
| Original Plan Element | Status | Required Change | New Approach |
|----------------------|--------|-----------------|--------------|
| Direct API integration | ❌ Invalid | Add data transformation layer | ETL pipeline + API adapter |
| Single data processing task | ❌ Insufficient | Split into 3 tasks | Data fetch, transform, validate |
| Task dependencies | ❌ Wrong order | Reorder based on new data flow | Data tasks before UI tasks |
```

## Assumption Testing Subagent Execution

### Assumption Testing Context Validation (Required First Step)
```bash
# Every assumption testing subagent must validate context access and testing mode
echo "Validating assumption testing context package..."
if [[ ! -d "./context" ]]; then
    echo "❌ FATAL: Context package not accessible"
    echo "Required: ./context/ directory with master analysis"
    exit 1
fi

# Validate assumption testing mode
if [[ ! -f "./ASSUMPTION_TESTING_MODE" ]]; then
    echo "❌ FATAL: Not in assumption testing mode"
    echo "This execution requires ASSUMPTION_TESTING_MODE file"
    exit 1
fi

echo "✅ Assumption testing mode confirmed"
cat ASSUMPTION_TESTING_MODE

# Initialize assumption testing log
test_category=$(basename $(pwd))
cat > assumption-testing-log.md << 'EOF'
# Assumption Testing Log: [Test Category]
**Started**: $(date)
**Worktree**: $(pwd)
**Mode**: ASSUMPTION_TESTING
**Test Category**: TEST_CATEGORY_PLACEHOLDER
**Status**: IN_PROGRESS

## Critical Assumptions Being Tested
### Primary Assumption
**Assumption**: [State the primary assumption being tested]
**Risk Level**: [HIGH/MEDIUM/LOW]
**Potential Impact**: [What happens to the plan if this is wrong?]

### Secondary Assumptions
[List related assumptions also being validated]

## Testing Approach
### Test Method
[How will the assumption be validated?]

### Success Criteria
[What constitutes validation of the assumption?]

### Failure Criteria  
[What constitutes invalidation of the assumption?]

### Alternative Approaches Ready
[List backup approaches to test if assumption fails]

## Assumption Testing Results
### Assumption 1: [Name]
- **Status**: [TESTING/VALIDATED/FAILED]
- **Evidence**: [What evidence supports the result?]
- **Plan Impact**: [How does this affect the overall plan?]
- **Alternative Needed**: [Yes/No - if yes, what alternative?]

### Assumption 2: [Name]
- **Status**: [TESTING/VALIDATED/FAILED]
- **Evidence**: [What evidence supports the result?]
- **Plan Impact**: [How does this affect the overall plan?]
- **Alternative Needed**: [Yes/No - if yes, what alternative?]

## Alternative Approach Testing
### When Primary Assumption Failed
[Document alternative approaches tested when assumptions proved false]

### Alternative 1: [Approach Name]
- **Description**: [What was tested?]
- **Result**: [Did this alternative work?]
- **Complexity**: [More/less complex than original approach?]
- **Plan Impact**: [How would this change the overall plan?]

### Alternative 2: [Approach Name]
- **Description**: [What was tested?]
- **Result**: [Did this alternative work?]
- **Complexity**: [More/less complex than original approach?]
- **Plan Impact**: [How would this change the overall plan?]

## AI Alternative Selection Framework
### Decision Criteria for Multiple AI Solutions
When AI agents test multiple approaches, use this framework to choose best solution:

#### 1. Feasibility Scoring (Must Pass)
- **Technical Feasibility**: Does it actually work with real constraints? (Pass/Fail)
- **Integration Feasibility**: Can it integrate with existing systems? (Pass/Fail)  
- **Resource Feasibility**: Can we implement within time/skill constraints? (Pass/Fail)

#### 2. Comparison Matrix (For Passing Solutions)
| Approach | Complexity Score | Performance Score | Maintenance Score | Total |
|----------|------------------|-------------------|-------------------|-------|
| Option A | Low (3) | High (3) | Medium (2) | 8 |
| Option B | Medium (2) | Medium (2) | High (3) | 7 |
| Option C | High (1) | High (3) | Low (1) | 5 |

**Scoring**: 3=Best, 2=Good, 1=Acceptable

#### 3. AI Testing Evidence Required
For each viable alternative, AI agents must provide:
- **Performance Evidence**: Actual numbers from testing
- **Complexity Evidence**: Lines of code, implementation time, dependencies
- **Integration Evidence**: Successful connection test with realistic data
- **Risk Evidence**: What could go wrong and how likely

#### 4. Selection Decision Tree
```
1. Filter by feasibility → Only passing solutions continue
2. If only one solution passes → Choose it
3. If multiple solutions pass:
   a. If scores within 1 point → Choose simplest (lowest complexity)
   b. If clear winner (2+ point difference) → Choose highest score
   c. If tie and complexity equal → Choose best performance
4. Document why other options were rejected
```

#### 5. AI Agent Coordination for Alternatives
```bash
# When assumption fails, spawn multiple AI agents with different approaches
test_assumption_alternatives() {
    local assumption_name="$1"
    local base_dir="~/work/worktrees/${feature_name}/proto"
    
    echo "Assumption failed: $assumption_name"
    echo "Spawning multiple AI agents to test alternatives..."
    
    # Spawn 3 AI agents with different approaches
    claude-code --worktree "${base_dir}/${assumption_name}-alt-1" "Test alternative approach 1: [specific method]. Focus on simplicity and quick implementation." &
    claude-code --worktree "${base_dir}/${assumption_name}-alt-2" "Test alternative approach 2: [specific method]. Focus on performance and scalability." &  
    claude-code --worktree "${base_dir}/${assumption_name}-alt-3" "Test alternative approach 3: [specific method]. Focus on integration and compatibility." &
    
    # Wait for all alternatives to complete
    wait
    
    # Compare results using decision framework
    compare_ai_alternatives "$assumption_name"
}
```

#### 6. Alternative Testing Time Limits
- **Per Alternative**: Maximum 4 hours of AI agent time
- **Total Testing**: Maximum 12 hours for all alternatives combined
- **Decision Time**: Maximum 1 hour to compare and choose
- **Documentation**: Maximum 30 minutes to document choice rationale

## Discoveries and Insights
### Unexpected Findings
[What was discovered that wasn't expected?]

### Technical Constraints Discovered
[New constraints/limitations found during testing]

### Integration Complexities
[Integration challenges not initially anticipated]

### Performance Characteristics
[Actual performance vs expected performance]

## Plan Adjustment Recommendations
### Tasks That Need Changes
- **Task**: [Task name]
- **Change Required**: [What needs to change?]
- **Reason**: [Why does it need to change?]
- **New Approach**: [Recommended new approach]

### New Tasks Required
- **New Task**: [Task name]
- **Purpose**: [Why is this task needed?]
- **Dependencies**: [What does it depend on?]
- **Complexity**: [Estimated complexity]

### Task Dependencies That Changed
- **Original Dependency**: [A depends on B]
- **New Dependency**: [A now depends on C and D]
- **Reason**: [Why did dependency change?]

### Timeline Impact
- **Original Estimate**: [Time estimate]
- **New Estimate**: [Updated time estimate]
- **Change Reason**: [Why did estimate change?]

## Validated Approach for Production
### Recommended Implementation Strategy
[The approach that should be used for production implementation]

### Critical Success Factors
[Key factors that must be in place for success]

### Known Risks and Mitigations
[Risks identified and how to mitigate them]

### Integration Requirements
[Specific requirements for integrating with other components]

## Assumption Testing Completion Status
- [ ] All primary assumptions tested
- [ ] All failed assumptions have working alternatives
- [ ] Plan impact fully documented
- [ ] Alternative approaches validated
- [ ] Production implementation strategy defined
- [ ] Task adjustments documented
- [ ] Timeline impact assessed
EOF

# Replace placeholder with actual test category
sed -i "s/TEST_CATEGORY_PLACEHOLDER/$test_category/" assumption-testing-log.md
sed -i "s/\\[Test Category\\]/$test_category/" assumption-testing-log.md

echo "✅ Assumption testing log initialized"
```

## Plan Adjustment Phase (CRITICAL)

### Processing Assumption Testing Results (Required Before Production)

#### 1. Aggregate Assumption Testing Results
```bash
# Collect all assumption testing results
aggregate_assumption_results() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    local assumptions_dir="requests/${feature_name}/assumptions"
    
    echo "=== Aggregating Assumption Testing Results ==="
    
    # Create results summary
    cat > "${assumptions_dir}/testing-results-summary.md" << EOF
# Assumption Testing Results Summary
**Completed**: $(date)

## Critical Findings Summary
### Assumptions Validated ✅
[List assumptions that were confirmed correct]

### Assumptions Failed ❌  
[List assumptions that proved incorrect]

### Alternative Approaches Validated ✅
[List alternative approaches that work]

### Plan Impact Assessment
**High Impact Changes Needed**: [Number]
**Medium Impact Changes Needed**: [Number] 
**Low Impact Changes Needed**: [Number]

EOF

    # Aggregate results from each testing area
    for proto_dir in ${proto_base}/*/; do
        test_category=$(basename "$proto_dir")
        echo "Processing results from: $test_category"
        
        if [[ -f "$proto_dir/assumption-testing-log.md" ]]; then
            echo "" >> "${assumptions_dir}/testing-results-summary.md"
            echo "## Results from $test_category" >> "${assumptions_dir}/testing-results-summary.md"
            
            # Extract key findings
            grep -A 10 "## Plan Adjustment Recommendations" "$proto_dir/assumption-testing-log.md" >> "${assumptions_dir}/testing-results-summary.md" || true
            
            echo "✅ Results extracted from $test_category"
        else
            echo "❌ No results found for $test_category"
        fi
    done
    
    echo "✅ Assumption testing results aggregated"
}

# Validate all assumptions tested before proceeding
validate_assumption_testing_complete() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    
    echo "=== Validating Assumption Testing Completion ==="
    
    local incomplete_count=0
    for proto_dir in ${proto_base}/*/; do
        if [[ ! -f "$proto_dir/ASSUMPTION_TESTING_COMPLETE" ]]; then
            echo "❌ Assumption testing incomplete: $(basename $proto_dir)"
            incomplete_count=$((incomplete_count + 1))
        fi
    done
    
    if [[ $incomplete_count -eq 0 ]]; then
        echo "✅ All assumption testing complete"
        return 0
    else
        echo "❌ $incomplete_count assumption tests incomplete"
        echo "Cannot proceed to plan adjustment until all testing complete"
        return 1
    fi
}
```

#### 2. Streamlined Plan Adjustment Process
```bash
# Simplified plan adjustment based on AI prototype findings
adjust_plan_from_ai_testing() {
    local feature_name="$1"
    local assumptions_dir="requests/${feature_name}/assumptions"
    
    echo "=== Streamlined Plan Adjustment from AI Testing ==="
    
    # Quick validation - all P0/P1 assumptions must be resolved
    validate_critical_assumptions_resolved "$feature_name" || exit 1
    
    # Generate plan diff (what changed vs original)
    cat > "${assumptions_dir}/plan-changes.md" << EOF
# Plan Changes from AI Assumption Testing
**Updated**: $(date)
**Testing Duration**: [Total hours spent on assumption testing]
**AI Agents Used**: [Number of parallel AI testing agents]

## SUMMARY: Plan Change Impact
- **Plan Change Magnitude**: [MINOR/MODERATE/MAJOR]
- **Task Count Change**: [Original: X → New: Y] 
- **Timeline Change**: [Original: X weeks → New: Y weeks]
- **Approach Change**: [Same/Modified/Completely Different]

## Quick Decision Matrix
| Assumption | Original Plan | AI Test Result | Plan Impact | New Approach |
|------------|---------------|----------------|-------------|--------------|
| API Data Format | Direct integration | ❌ Missing fields | Add transform layer | ETL pipeline |
| Performance Target | Single query | ❌ Too slow | Add caching | Redis + batch |
| Integration Pattern | REST API | ✅ Works | No change | Keep REST |

## Task Adjustments (Find/Replace Style)
### Tasks to Split
- **Original**: "Implement user data processing" 
- **Split Into**: "Fetch user data" + "Transform data format" + "Validate processed data"
- **Reason**: AI testing revealed data transformation complexity

### Tasks to Merge  
- **Original**: "Setup auth" + "Test auth" + "Integrate auth"
- **Merged Into**: "Implement auth system"
- **Reason**: AI testing showed this is simpler than expected

### Tasks to Remove
- **Removed**: "Build custom data parser"
- **Reason**: AI found existing library that works better

### New Tasks Required
- **Added**: "Implement Redis caching layer"
- **Reason**: AI performance testing revealed need for caching

## Integration with subtasks-plan.md
### Simple Approach Changes
1. **Replace** original technical approach with AI-validated approach
2. **Update** complexity estimates based on AI testing results  
3. **Adjust** task dependencies based on what AI testing revealed
4. **Re-run** subtasks-plan.md with updated approach description

### Updated Approach Description for Planning
\`\`\`
Use this validated approach when re-running subtasks-plan.md:

**Technical Stack**: [Based on AI testing results]
**Data Flow**: [Validated by AI integration testing]  
**Performance Strategy**: [Proven by AI performance testing]
**Integration Pattern**: [Validated by AI integration testing]
**Risk Mitigation**: [Based on AI risk testing results]
\`\`\`
EOF

    echo "✅ Streamlined plan changes documented"
}

# Quick validation that critical assumptions are resolved
validate_critical_assumptions_resolved() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    
    echo "=== Validating Critical Assumptions Resolved ==="
    
    local unresolved_count=0
    
    # Check P0 and P1 assumptions have working solutions
    for proto_dir in ${proto_base}/*/; do
        if [[ -f "$proto_dir/ASSUMPTION_TESTING_COMPLETE" ]]; then
            # Check if any HIGH risk assumptions still failed without alternatives
            if grep -q "Status.*FAILED" "$proto_dir/assumption-testing-log.md" && \
               ! grep -q "Alternative.*VALIDATED" "$proto_dir/assumption-testing-log.md"; then
                echo "❌ Critical assumption unresolved: $(basename $proto_dir)"
                unresolved_count=$((unresolved_count + 1))
            fi
        fi
    done
    
    if [[ $unresolved_count -eq 0 ]]; then
        echo "✅ All critical assumptions resolved with working alternatives"
        return 0
    else
        echo "❌ $unresolved_count critical assumptions still unresolved"
        echo "Cannot proceed to production until all P0/P1 assumptions have working solutions"
        return 1
    fi
}
# Updated Plan Based on Assumption Testing
**Updated**: $(date)
**Original Plan**: requests/${feature_name}/plan.md
**Assumption Testing Results**: requests/${feature_name}/assumptions/testing-results-summary.md

## Plan Changes Required

### Tasks That Need Modification
[Based on assumption testing findings]

#### Original Task: [Task Name]
- **Original Scope**: [What was originally planned]
- **Assumption Impact**: [Which failed assumption affects this]
- **Required Changes**: [How scope/approach must change]
- **New Complexity**: [Updated complexity estimate]
- **New Dependencies**: [Updated dependencies]

### New Tasks Required
[Tasks that must be added due to assumption testing discoveries]

#### New Task: [Task Name]
- **Purpose**: [Why this task is now needed]
- **Discovered By**: [Which assumption test revealed this need]
- **Dependencies**: [What this depends on]
- **Complexity**: [Estimated complexity]
- **Priority**: [Where this fits in sequence]

### Tasks That Can Be Removed/Simplified
[Tasks no longer needed or simplified by assumption testing]

#### Removed Task: [Task Name]
- **Reason**: [Why no longer needed]
- **Alternative Approach**: [What replaces this]

### Updated Dependencies and Sequencing
[How task dependencies changed based on findings]

#### Original Sequence
1. [Original task order]

#### Updated Sequence  
1. [New task order based on validated approach]

### Updated Timeline and Complexity
- **Original Estimate**: [Total time]
- **Updated Estimate**: [New total time]
- **Change Factor**: [Increase/decrease and why]

## Validated Technical Approach
### Architecture Changes
[Architecture changes needed based on assumption testing]

### Technology Stack Adjustments
[Any technology changes needed]

### Integration Pattern Changes
[How components will integrate differently]

### Performance Strategy
[Performance approach based on testing results]

## Risk Mitigation Updates
### Risks Eliminated by Testing
[Risks that assumption testing eliminated]

### New Risks Discovered
[New risks found during assumption testing]

### Updated Risk Mitigation Strategies
[How to handle discovered risks]

## Context Package Updates Needed
### Implementation Patterns to Add
[New patterns discovered during assumption testing]

### External Sources to Update
[New external documentation needed]

### Updated Integration Guidelines
[How integration approach changed]
EOF

    echo "✅ Updated plan document created"
}

# Update task files based on assumption testing findings
update_tasks_from_assumptions() {
    local feature_name="$1"
    local assumptions_dir="requests/${feature_name}/assumptions"
    local tasks_dir="requests/${feature_name}/tasks"
    
    echo "=== Updating Task Files Based on Assumptions ==="
    
    # Archive original task files
    mkdir -p "${tasks_dir}/original-tasks-backup-$(date +%Y%m%d)"
    cp -r "${tasks_dir}"/*.md "${tasks_dir}/original-tasks-backup-$(date +%Y%m%d)/" 2>/dev/null || true
    
    # Generate updated task files
    # This would integrate with the subtasks-plan.md process
    # but use the validated approach from assumption testing
    
    echo "🔄 Task file regeneration required"
    echo "Use updated-plan.md to regenerate task files with subtasks-plan.md"
    echo "Incorporate validated approaches from assumption testing"
    
    # Create task update checklist
    cat > "${assumptions_dir}/task-update-checklist.md" << EOF
# Task Update Checklist
**Updated**: $(date)

## Required Actions
- [ ] **Review updated-plan.md**: Understand all plan changes
- [ ] **Run subtasks-plan.md**: Regenerate tasks with updated approach
- [ ] **Update context package**: Include assumption testing findings
- [ ] **Validate new task dependencies**: Ensure correct sequencing
- [ ] **Update timeline estimates**: Reflect assumption testing discoveries

## Integration with subtasks-plan.md
1. Use validated technical approach from assumption testing
2. Incorporate complexity discoveries from prototypes
3. Use validated integration patterns
4. Apply risk mitigation strategies discovered
5. Update external dependency handling based on testing

## Context Package Updates Required
- Add validated implementation patterns
- Update integration guidelines  
- Include performance characteristics discovered
- Add alternative approaches for risk mitigation
- Update external dependency handling strategies
EOF

    echo "✅ Task update checklist created"
}
```

#### 3. Assumption Testing Completion Process
```bash
# Complete assumption testing phase and transition to production planning
complete_assumption_testing_phase() {
    local feature_name="$1"
    
    echo "=== Completing Assumption Testing Phase ==="
    
    # Aggregate all results
    aggregate_assumption_results "$feature_name"
    
    # Regenerate plan based on findings
    regenerate_plan_from_assumptions "$feature_name"
    
    # Update task files
    update_tasks_from_assumptions "$feature_name"
    
    # Create phase completion report
    cat > "requests/${feature_name}/assumptions/phase-completion-report.md" << EOF
# Assumption Testing Phase Completion Report
**Completed**: $(date)

## Phase Summary
- **Total Assumptions Tested**: [Number]
- **Assumptions Validated**: [Number]
- **Assumptions Failed**: [Number] 
- **Alternative Approaches Found**: [Number]
- **Plan Impact**: [HIGH/MEDIUM/LOW]

## Key Discoveries
### Critical Findings
[Most important discoveries that changed the plan]

### Technical Insights
[Key technical discoveries for production implementation]

### Complexity Changes
[How complexity estimates changed]

### Risk Profile Changes
[How project risk changed based on testing]

## Production Readiness
### Validated Approach
[The approach that will be used for production]

### Updated Plan Status
- [ ] **Plan Updated**: Based on assumption testing results
- [ ] **Tasks Regenerated**: Using validated approach
- [ ] **Context Updated**: Including testing insights
- [ ] **Dependencies Validated**: Correct sequencing confirmed
- [ ] **Timeline Updated**: Reflecting new complexity understanding

### Next Steps
1. **Review Updated Plan**: Validate the updated plan makes sense
2. **Regenerate Production Tasks**: Use subtasks-plan.md with validated approach  
3. **Create Production Worktrees**: Set up final implementation worktrees
4. **Begin Production Implementation**: With confidence in the approach

## Risk Assessment for Production
### Risks Eliminated
[Risks that assumption testing eliminated]

### Remaining Risks
[Risks still present for production phase]

### New Risks Discovered
[Risks found during assumption testing]

### Mitigation Strategies
[How to handle remaining and new risks]
EOF

    # Mark assumption testing phase complete
    touch "requests/${feature_name}/ASSUMPTION_TESTING_COMPLETE"
    
    echo "✅ Assumption testing phase marked complete"
    echo "Next: Review updated plan and regenerate production tasks"
}
```

### Assumption Testing Implementation Process

#### 1. Rapid Assumption Testing Guidelines
```markdown
## AI Assumption Testing Scope Boundaries
### "Good Enough" Criteria for AI Testing:
- **Functional Evidence**: Core assumption works/fails with real data (not perfect implementation)
- **Performance Evidence**: Rough performance characteristics measured (±50% accuracy acceptable)
- **Integration Evidence**: Components can connect (doesn't need full error handling)
- **Complexity Evidence**: Implementation effort estimate (2x complexity change threshold)

### AI Testing Scope Limits (Prevent Over-Engineering):
- **Build Minimum Viable Test**: 50-200 lines max per assumption test
- **Use Shortcuts Liberally**: Hardcode configs, mock auth, skip edge cases
- **No Production Quality**: No error handling, logging, tests, or documentation
- **Time-Box Strictly**: 2-8 hours max per assumption (AI should be faster)
- **Evidence Over Implementation**: Care about what you learn, not what you build

### AI Testing "Stop Criteria":
- **Assumption Validated**: Evidence clearly supports original assumption → Stop
- **Assumption Failed + Alternative Found**: Working alternative discovered → Stop  
- **Time Limit Reached**: Hit max time limit → Document findings and stop
- **Blocking Dependency**: Can't test due to external factor → Document and move on

### AI Speed Advantage Rules:
- **Parallel Testing**: Run 2-3 AI agents on same assumption with different approaches
- **Rapid Iteration**: If approach fails in 30 minutes, try different approach
- **Quick Pivots**: Don't debug broken approach, try alternative approach
- **Disposable Code**: Delete test code after extracting lessons learned

### Quality Through Quantity Strategy:
- **Multiple AI Approaches**: Test 3-5 different solutions to same problem
- **Comparative Analysis**: Best approach emerges from comparison, not perfection
- **Fast Failure Detection**: AI can quickly identify non-viable approaches
- **Validated Patterns**: Final implementation uses proven approach from testing

### Evidence Collection Focus:
- **Performance Numbers**: Actual response times, memory usage, throughput
- **Integration Success/Failure**: Can components communicate? What breaks?
- **Data Format Validation**: Does real data match expected format?
- **Library/Service Capabilities**: What do external dependencies actually provide?
- **Complexity Discoveries**: What's harder/easier than expected?
```

### Learning Focus:
- Validate core technical assumptions
- Test integration points thoroughly
- Measure performance characteristics  
- Identify unexpected complexity
- Document surprises and insights

### Prototype Quality Standards:
- **Must Work**: Core functionality must demonstrate feasibility
- **Must Integrate**: Interfaces must connect to other components
- **Must Measure**: Performance characteristics must be observable
- **Must Document**: Lessons must be captured for production
```

#### 2. Prototype Validation Commands
```bash
# Prototype-specific validation (less strict than production)
validate_prototype() {
    echo "=== Prototype Validation ==="
    
    # Core functionality test (must pass)
    if npm run test:core 2>/dev/null; then
        echo "✅ Core functionality working"
    else
        echo "❌ Core functionality failed - prototype not viable"
        return 1
    fi
    
    # Integration test (must pass)
    if npm run test:integration 2>/dev/null; then
        echo "✅ Integration points working"
    else
        echo "❌ Integration failed - needs architecture review"
        return 1
    fi
    
    # Performance test (measure, don't fail)
    if npm run test:performance 2>/dev/null; then
        echo "✅ Performance characteristics measured"
    else
        echo "⚠️ Performance not measured - add to production requirements"
    fi
    
    # Code quality (warn, don't fail)
    if npm run lint 2>/dev/null; then
        echo "✅ Code quality acceptable"
    else
        echo "⚠️ Code quality issues - address in production"
    fi
    
    echo "✅ Prototype validation complete"
}
```

#### 3. Prototype Completion Process
```bash
# Complete prototype and prepare lessons for production
complete_prototype() {
    echo "=== Completing Prototype Phase ==="
    
    # Validate prototype meets success criteria
    validate_prototype || {
        echo "❌ Prototype validation failed"
        echo "Cannot proceed to production planning"
        exit 1
    }
    
    # Create lessons learned document
    cat > LESSONS_LEARNED.md << EOF
# Lessons Learned: $(basename $(pwd))

## Prototype Summary
**Completion Date**: $(date)
**Prototype Duration**: [Time spent on prototype]
**Success Status**: ✅ COMPLETE

## Technical Discoveries
### What Works Well
[List approaches/patterns that work and should be kept in production]

### What Needs Change
[List approaches that need different implementation in production]

### Unexpected Complexity
[Areas that proved more complex than estimated]

### Unexpected Simplicity  
[Areas that proved simpler than estimated]

## Performance Characteristics
### Observed Performance
[Actual performance measurements from prototype]

### Performance Bottlenecks
[Areas that will need optimization in production]

### Scalability Concerns
[Issues that will impact production scalability]

## Integration Insights
### Interface Validation
[How well do the planned interfaces work?]

### Integration Challenges
[Problems discovered in connecting to other components]

### Communication Patterns
[Effective patterns for component communication]

## Production Recommendations
### Architecture Changes
[Recommended changes to overall system architecture]

### Implementation Strategy
[Recommended approach for production development]

### Time Estimate Adjustments
[Updated estimates based on prototype complexity]

### Risk Mitigation Updates
[Updated strategies based on prototype risk validation]

### Technology Stack Adjustments
[Any recommended changes to technology choices]

## Production Task Updates
### Scope Changes
[How should production task scope be adjusted?]

### New Requirements
[Requirements discovered during prototyping]

### Dependency Changes
[How do prototype findings affect task dependencies?]

### Resource Requirements
[Updated resource needs for production implementation]

## Context Package Updates Needed
### New Patterns Discovered
[Patterns found during prototyping that should be added to context]

### Updated External Sources
[New external documentation/resources discovered]

### Revised Implementation Strategies
[Changes to recommended implementation approaches]

## Critical Production Notes
### Must Fix for Production
[Issues that MUST be addressed in production implementation]

### Production Blockers
[Any blockers discovered that could impact production]

### Integration Requirements
[Specific requirements for production integration]

### Testing Requirements
[Testing strategies needed for production based on prototype learnings]
EOF

    # Mark prototype complete
    touch PROTOTYPE_COMPLETE
    echo "✅ Prototype marked complete"
    
    # Update prototype status
    echo "COMPLETE: $(date) - Lessons documented in LESSONS_LEARNED.md" > status.txt
    
    echo "✅ Prototype completion process finished"
}
```

## Production Phase Execution

### Production Worktree Creation (After All Prototypes Complete)
```bash
# Create production worktrees with prototype lessons
create_production_worktrees() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    local final_base="~/work/worktrees/${feature_name}/final"
    
    echo "=== Creating Production Worktrees ==="
    
    # Validate all prototypes are complete
    for proto_dir in ${proto_base}/*/; do
        if [[ ! -f "$proto_dir/PROTOTYPE_COMPLETE" ]]; then
            echo "❌ Prototype incomplete: $(basename $proto_dir)"
            echo "Cannot create production worktrees until all prototypes complete"
            exit 1
        fi
    done
    
    echo "✅ All prototypes complete, creating production worktrees"
    
    # Create production worktrees
    for proto_dir in ${proto_base}/*/; do
        task_name=$(basename "$proto_dir")
        
        # Create production branch and worktree  
        git worktree add "${final_base}/${task_name}" -b "feature/${feature_name}-${task_name}"
        
        # Copy lessons learned and updated context
        cp "$proto_dir/LESSONS_LEARNED.md" "${final_base}/${task_name}/"
        cp "$proto_dir/task.md" "${final_base}/${task_name}/"
        ln -sf "$(pwd)/requests/${feature_name}/context" "${final_base}/${task_name}/context"
        
        # Mark as production environment
        cat > "${final_base}/${task_name}/PRODUCTION_MODE" << EOF
# PRODUCTION ENVIRONMENT  
This is a production worktree for final implementation.

## Production Goals:
- Production-quality code
- Complete error handling
- Full test coverage
- Performance optimization
- Security compliance

## Prototype Lessons Available:
- LESSONS_LEARNED.md contains prototype insights
- Use prototype findings to guide implementation
- Avoid prototype pitfalls identified

## Success Criteria:
- Production-ready code quality
- All tests passing
- Performance requirements met
- Security requirements met
- Integration fully validated
EOF
        
        echo "✅ Production worktree created: ${final_base}/${task_name}"
    done
    
    echo "✅ All production worktrees ready"
}
```

### Production Context Update Process
```bash
# Update context package with prototype lessons
update_context_from_prototypes() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    local context_dir="requests/${feature_name}/context"
    
    echo "=== Updating Context Package with Prototype Lessons ==="
    
    # Aggregate lessons learned from all prototypes
    cat > "${context_dir}/prototype-lessons.md" << EOF
# Prototype Lessons Learned Summary

## Consolidated Findings from All Prototypes
**Update Date**: $(date)

EOF

    # Aggregate lessons from each prototype
    for proto_dir in ${proto_base}/*/; do
        task_name=$(basename "$proto_dir")
        if [[ -f "$proto_dir/LESSONS_LEARNED.md" ]]; then
            echo "## Lessons from $task_name" >> "${context_dir}/prototype-lessons.md"
            cat "$proto_dir/LESSONS_LEARNED.md" >> "${context_dir}/prototype-lessons.md"
            echo "" >> "${context_dir}/prototype-lessons.md"
        fi
    done
    
    # Update implementation patterns with prototype findings
    cat >> "${context_dir}/implementation-patterns.md" << EOF

## Updated Patterns from Prototype Phase
**Added**: $(date)

### Validated Patterns
[Patterns confirmed to work well by prototypes]

### Revised Patterns  
[Patterns that need modification based on prototype findings]

### New Patterns Discovered
[New patterns discovered during prototyping]

### Anti-Patterns Identified
[Approaches that prototypes showed don't work well]
EOF

    echo "✅ Context package updated with prototype lessons"
}
```

### Production Subagent Execution
```bash
# Production subagent initialization (after prototype lessons)
initialize_production_execution() {
    echo "Validating production context and lessons..."
    
    # Validate production mode
    if [[ ! -f "./PRODUCTION_MODE" ]]; then
        echo "❌ FATAL: Not in production mode"
        exit 1
    fi
    
    # Validate lessons learned available
    if [[ ! -f "./LESSONS_LEARNED.md" ]]; then
        echo "❌ FATAL: Prototype lessons not available"
        exit 1
    fi
    
    echo "✅ Production mode confirmed"
    echo "✅ Prototype lessons available"
    
    # Initialize production execution log
    task_name=$(basename $(pwd))
    cat > production-log.md << 'EOF'
# Production Execution Log: [Task Name]
**Started**: $(date)
**Worktree**: $(pwd)
**Mode**: PRODUCTION
**Task ID**: TASK_ID_PLACEHOLDER
**Status**: IN_PROGRESS

## Prototype Lessons Applied
### Key Insights Used
[List insights from LESSONS_LEARNED.md that influenced implementation]

### Prototype Pitfalls Avoided
[List problems identified in prototype that were avoided]

### Architecture Changes Applied
[Changes made based on prototype recommendations]

## Production Implementation Tracking
### Quality Standards Applied
[Track production-quality practices used]

### Performance Optimizations
[Track performance improvements over prototype]

### Security Measures
[Track security implementations]

### Error Handling
[Track comprehensive error handling]

### Test Coverage
[Track test implementation and coverage]

## Production Validation Results
### All Tests Passing
[Track comprehensive test suite results]

### Performance Requirements Met
[Track performance validation against requirements]

### Security Requirements Met
[Track security validation]

### Integration Fully Validated
[Track thorough integration testing]

## Production Completion Status
- [ ] All production quality standards met
- [ ] Complete test coverage implemented
- [ ] Performance requirements validated
- [ ] Security requirements validated
- [ ] Error handling comprehensive
- [ ] Integration thoroughly tested
- [ ] Documentation complete
- [ ] Ready for production deployment
EOF

    # Replace placeholder
    sed -i "s/TASK_ID_PLACEHOLDER/$task_name/" production-log.md
    sed -i "s/\\[Task Name\\]/$task_name/" production-log.md
    
    echo "✅ Production execution log initialized"
}

# Production validation (stricter than prototype)
validate_production() {
    echo "=== Production Validation ==="
    
    # All tests must pass
    npm run test && echo "✅ All tests passing" || {
        echo "❌ Tests failing - cannot complete production"
        return 1
    }
    
    # Lint must pass
    npm run lint && echo "✅ Code quality standards met" || {
        echo "❌ Linting failed - fix before completion"
        return 1
    }
    
    # Performance requirements must be met
    npm run test:performance && echo "✅ Performance requirements met" || {
        echo "❌ Performance requirements not met"
        return 1
    }
    
    # Integration tests must pass
    npm run test:integration && echo "✅ Integration fully validated" || {
        echo "❌ Integration tests failing"
        return 1
    }
    
    echo "✅ Production validation complete"
}
```

## Integration and Completion

### Production Task Completion Process
```bash
# Complete production task
complete_production_task() {
    echo "=== Completing Production Task ==="
    
    # Validate production meets all requirements
    validate_production || {
        echo "❌ Production validation failed"
        exit 1
    }
    
    # Create completion report comparing prototype to production
    cat > PRODUCTION_COMPLETE.md << EOF
# Production Completion Report: $(basename $(pwd))

## Implementation Summary
**Completion Date**: $(date)
**Total Duration**: [Prototype + Production time]
**Status**: ✅ PRODUCTION READY

## Prototype vs Production Comparison
### Prototype Duration vs Production Duration
- **Prototype**: [Time spent]
- **Production**: [Time spent]  
- **Total**: [Combined time]
- **Efficiency**: [Production faster due to prototype learnings?]

### Scope Changes from Prototype
[How did production scope differ from original prototype scope?]

### Quality Improvements
[Improvements made in production over prototype]

### Performance Improvements
[Performance gains from prototype to production]

## Prototype Value Assessment
### Time Saved by Prototyping
[Estimate of time saved by prototyping first]

### Risks Mitigated by Prototyping
[Risks that prototype helped avoid in production]

### Architecture Improvements
[Better architecture decisions made due to prototype insights]

### Integration Benefits
[Integration problems avoided due to prototype testing]

## Final Production Status
### Quality Metrics
- **Test Coverage**: [Percentage]
- **Performance**: [Meets requirements]
- **Security**: [Validated]
- **Error Handling**: [Comprehensive]

### Integration Status
- **Interfaces**: [All validated]
- **Dependencies**: [All met]
- **Cross-component**: [All tested]

### Production Readiness
- **Deployment Ready**: ✅
- **Monitoring Ready**: ✅
- **Documentation Complete**: ✅
- **Team Handoff Ready**: ✅
EOF

    # Mark production complete
    touch PRODUCTION_COMPLETE
    echo "✅ Production task marked complete"
}
```

### Final Branch Management
```bash
# Merge production worktrees to feature branches (same as before)
merge_production_to_branches() {
    local feature_name="$1"
    local final_base="~/work/worktrees/${feature_name}/final"
    
    echo "=== Merging Production Worktrees to Feature Branches ==="
    
    for prod_dir in ${final_base}/*/; do
        if [[ -f "$prod_dir/PRODUCTION_COMPLETE" ]]; then
            cd "$prod_dir"
            task_name=$(basename "$prod_dir")
            
            # Ensure all changes committed
            if [[ -n $(git status --porcelain) ]]; then
                echo "❌ Uncommitted changes in: $prod_dir"
                exit 1
            fi
            
            # Feature branch already created, just push
            git push -u origin "feature/${feature_name}-${task_name}"
            echo "✅ Production merged to branch: feature/${feature_name}-${task_name}"
        fi
    done
}

# Cleanup prototype worktrees after production complete
cleanup_prototypes() {
    local feature_name="$1"
    local proto_base="~/work/worktrees/${feature_name}/proto"
    
    echo "=== Cleaning up Prototype Worktrees ==="
    
    # Archive prototype lessons before cleanup
    mkdir -p "docs/prototypes/${feature_name}"
    for proto_dir in ${proto_base}/*/; do
        task_name=$(basename "$proto_dir")
        cp "$proto_dir/LESSONS_LEARNED.md" "docs/prototypes/${feature_name}/${task_name}-lessons.md"
        cp "$proto_dir/prototype-log.md" "docs/prototypes/${feature_name}/${task_name}-log.md"
    done
    
    # Remove prototype worktrees
    for proto_dir in ${proto_base}/*/; do
        git worktree remove "$proto_dir"
        echo "✅ Removed prototype worktree: $proto_dir"
    done
    
    # Delete prototype branches
    for branch in $(git branch | grep "proto/${feature_name}-"); do
        git branch -D "$branch"
        echo "✅ Deleted prototype branch: $branch"
    done
    
    echo "✅ Prototype cleanup complete"
}
```

## Final Execution Checklist

```markdown
## Assumption-Testing-First Task Master Final Checklist (MANDATORY)

### Assumption Testing Phase Complete:
- [ ] **All Critical Assumptions Tested**: Every high-risk assumption validated or alternative found
- [ ] **Testing Results Documented**: All assumption testing logs complete
- [ ] **Plan Updated**: Overall plan adjusted based on testing findings
- [ ] **Task Files Regenerated**: New task files created with validated approach
- [ ] **Context Updated**: Context package includes assumption testing insights

### Production Phase Complete:
- [ ] **All Production Tasks Complete**: Every production worktree has PRODUCTION_COMPLETE file
- [ ] **Feature Branches Created**: Each production worktree merged to feature/name-task branch
- [ ] **Branches Pushed**: All feature branches pushed to origin
- [ ] **Workplan Committed**: Complete workplan with assumption testing insights committed to base branch
- [ ] **Assumption Testing Archived**: Testing results preserved in docs/assumptions/

### Final Validation:
```bash
# Execute final validation for assumption-testing-first workflow
validate_assumption_testing_production_complete() {
    local feature_name="$1"
    
    echo "=== Assumption-Testing-First Completion Validation ==="
    
    # Check assumption testing phase complete
    if [[ ! -f "requests/${feature_name}/ASSUMPTION_TESTING_COMPLETE" ]]; then
        echo "❌ Assumption testing phase not complete"
        exit 1
    fi
    
    # Check plan was updated based on assumptions
    if [[ ! -f "requests/${feature_name}/assumptions/updated-plan.md" ]]; then
        echo "❌ Plan not updated based on assumption testing"
        exit 1
    fi
    
    # Check production tasks complete
    local incomplete_count=0
    for prod_dir in ~/work/worktrees/${feature_name}/final/*/; do
        if [[ ! -f "$prod_dir/PRODUCTION_COMPLETE" ]]; then
            echo "❌ Production incomplete: $(basename $prod_dir)"
            incomplete_count=$((incomplete_count + 1))
        fi
    done
    
    # Check feature branches exist
    for prod_dir in ~/work/worktrees/${feature_name}/final/*/; do
        task_name=$(basename "$prod_dir")
        if ! git branch -r | grep -q "feature/${feature_name}-${task_name}"; then
            echo "❌ Feature branch missing: feature/${feature_name}-${task_name}"
            incomplete_count=$((incomplete_count + 1))
        fi
    done
    
    # Check assumption testing results archived
    if [[ ! -d "docs/assumptions/${feature_name}" ]]; then
        echo "❌ Assumption testing results not archived"
        incomplete_count=$((incomplete_count + 1))
    fi
    
    if [[ $incomplete_count -eq 0 ]]; then
        echo "✅ ASSUMPTION-TESTING-FIRST WORKFLOW COMPLETE"
        echo "All production tasks ready for integration review"
        echo "Plan successfully adjusted based on validated assumptions"
    else
        echo "❌ $incomplete_count issues found"
        exit 1
    fi
}
```

## Workflow Summary

### Three-Phase Process:
1. **Assumption Testing Phase**: Test critical assumptions, discover alternatives when assumptions fail
2. **Plan Adjustment Phase**: Update overall plan based on testing results, regenerate tasks
3. **Production Implementation Phase**: Implement with confidence in validated approach

### Key Success Factors:
- **Assumption-Driven**: Focus on testing assumptions that could invalidate plan
- **Alternative-Ready**: Prepared to test multiple approaches when assumptions fail
- **Plan-Adaptive**: Plan changes based on evidence from assumption testing
- **Risk-Mitigated**: Major risks identified and mitigated before production

## MANDATORY: Prototype-First Execution Completion Results and Handoff

### Critical Completion Requirements

**⚠️ IMPORTANT**: When prototype-first execution completes, the Task Master MUST provide explicit verification of all results before handoff. Future agents will start with ZERO context from this execution session.

#### 1. Complete Results Verification Across All Phases (REQUIRED)
```bash
# MANDATORY: Verify results from assumption testing AND production phases
echo "=== PROTOTYPE-FIRST EXECUTION RESULTS VERIFICATION ==="

# Verify assumption testing phase results
echo "\n🔍 ASSUMPTION TESTING PHASE RESULTS:"
if [[ -f "requests/<feature>/ASSUMPTION_TESTING_COMPLETE" ]]; then
    echo "Status: ✅ COMPLETE"
    echo "Results archive: docs/assumptions/<feature>/"
    
    echo "\nCritical assumptions tested:"
    for proto_dir in ~/work/worktrees/<feature>/proto/*/; do
        if [[ -f "$proto_dir/assumption-testing-log.md" ]]; then
            echo "  🧪 $(basename "$proto_dir"): $(grep -o 'Status.*' "$proto_dir/assumption-testing-log.md" | head -1)"
        fi
    done
else
    echo "Status: ❌ INCOMPLETE - CRITICAL ERROR"
fi

# Verify production phase results
echo "\n🏠 PRODUCTION PHASE RESULTS:"
for prod_dir in ~/work/worktrees/<feature>/final/*/; do
    task_name=$(basename "$prod_dir")
    echo "\n📋 Production Task: $task_name"
    echo "Location: $prod_dir"
    
    if [[ -f "$prod_dir/PRODUCTION_COMPLETE" ]]; then
        echo "Status: ✅ COMPLETE"
        
        cd "$prod_dir"
        echo "Files modified:"
        git status --porcelain | while read status file; do
            echo "  $status $file"
        done
        
        current_branch=$(git branch --show-current)
        echo "Feature branch: $current_branch"
        echo "Commits: $(git rev-list --count HEAD ^main 2>/dev/null || echo 'N/A')"
    else
        echo "Status: ❌ INCOMPLETE"
    fi
done

echo "\n=== PLAN ADJUSTMENT VERIFICATION ==="
if [[ -f "requests/<feature>/assumptions/updated-plan.md" ]]; then
    echo "Updated plan: ✅ EXISTS"
    echo "Plan changes documented based on assumption testing results"
else
    echo "Updated plan: ❌ MISSING - CRITICAL ERROR"
fi
```

#### 2. Comprehensive Results Summary for User (REQUIRED)
```markdown
## PROTOTYPE-FIRST EXECUTION COMPLETION SUMMARY

### Assumption Testing Results
**Testing Archive**: `docs/assumptions/<feature>/`
**Status**: ✅ Complete / ❌ Incomplete

#### Critical Assumptions Tested
1. **[Assumption Name]**: [VALIDATED/FAILED - Alternative Found]
   - **Evidence**: [Key findings from testing]
   - **Impact**: [How this affected the plan]
   - **Alternative Used**: [If assumption failed, what worked instead]

2. **[Assumption Name]**: [VALIDATED/FAILED - Alternative Found]
   - **Evidence**: [Key findings from testing]
   - **Impact**: [How this affected the plan]
   - **Alternative Used**: [If assumption failed, what worked instead]

[Continue for all critical assumptions...]

#### Plan Changes from Assumption Testing
- **Original Approach**: [Brief description of original plan]
- **Validated Approach**: [What the testing proved works]
- **Task Count Change**: [Original X tasks → Final Y tasks]
- **Timeline Change**: [Original estimate → Updated estimate]
- **Complexity Change**: [What proved easier/harder than expected]

### Production Implementation Results
**Production Worktrees**: `~/work/worktrees/<feature>/final/`

#### Production Task 1: [Name]
- **Location**: `/absolute/path/to/production/worktree1/`
- **Status**: ✅ Complete / ❌ Incomplete
- **Files Modified**: [List key files with absolute paths]
- **Feature Branch**: `feature/<feature>-task1`
- **Prototype Lessons Applied**: [Key insights from assumption testing that guided implementation]

#### Production Task 2: [Name]
- **Location**: `/absolute/path/to/production/worktree2/`
- **Status**: ✅ Complete / ❌ Incomplete
- **Files Modified**: [List key files with absolute paths]
- **Feature Branch**: `feature/<feature>-task2`
- **Prototype Lessons Applied**: [Key insights from assumption testing that guided implementation]

[Continue for all production tasks...]

### Verification Commands for User
**User can verify all results with these commands:**
```bash
# Verify assumption testing phase results
ls -la docs/assumptions/<feature>/
cat docs/assumptions/<feature>/testing-results-summary.md

# Check updated plan based on assumption testing
cat requests/<feature>/assumptions/updated-plan.md

# Verify production worktrees and results
for worktree in ~/work/worktrees/<feature>/final/*/; do
    echo "Checking: $worktree"
    cd "$worktree" && git status && git log --oneline -3
done

# Check all feature branches created
git branch -r | grep "feature/<feature>-"

# Verify workplan with assumption testing insights
ls -la docs/workplans/<feature>/
cat docs/workplans/<feature>/completion.md
```

### Current State Summary
**What exists now after prototype-first execution:**
- ✅ **Assumption Testing Complete**: All critical assumptions validated or alternatives found
- ✅ **Plan Updated**: Implementation plan adjusted based on testing evidence
- ✅ **Production Worktrees**: [Number] worktrees with validated implementations
- ✅ **Feature Branches**: [Number] feature branches with production-ready code
- ✅ **Workplan Archive**: Complete workplan with assumption testing insights preserved
- ⏳ **Next Required**: Integration review of validated implementation

### Next Steps Required
1. **Immediate**: Review assumption testing results and plan changes
2. **Validation**: Confirm production implementation matches validated approach
3. **Integration**: Merge feature branches based on assumption-tested architecture
4. **Testing**: Run integration tests using patterns validated in assumption testing
5. **Cleanup**: Remove prototype worktrees, keep production branches

### For Future Agents
**⚠️ CRITICAL**: Any future agent will need this context:
- **Feature Scope**: [Brief description of overall feature after plan adjustment]
- **Validated Approach**: [Technical approach proven by assumption testing]
- **Key Assumptions Tested**: [List of critical assumptions and their outcomes]
- **Plan Changes Made**: [How original plan was adjusted based on testing]
- **Production Worktrees**: `~/work/worktrees/<feature>/final/` with validated implementations
- **Feature Branches**: All branches prefixed with `feature/<feature>-` contain assumption-validated code
- **Context Archives**: Complete context in `docs/workplans/<feature>/` and `docs/assumptions/<feature>/`
- **Integration Strategy**: [How components should integrate based on assumption testing]
- **Outstanding Work**: [Any remaining work based on validated plan]
```

#### 3. Branch and Archive State Documentation (REQUIRED)
```bash
# MANDATORY: Document complete state including assumption testing archives
echo "=== PROTOTYPE-FIRST EXECUTION STATE ==="
echo "Base branch: $(git branch --show-current)"
echo "Assumption testing archive: docs/assumptions/<feature>/"
echo "Updated workplan: docs/workplans/<feature>/"

echo "\nAssertion testing completion:"
if [[ -f "requests/<feature>/ASSUMPTION_TESTING_COMPLETE" ]]; then
    echo "  ✅ Assumption testing phase complete"
else
    echo "  ❌ Assumption testing phase incomplete - CRITICAL"
fi

echo "\nProduction feature branches:"
for branch in $(git branch -r | grep "feature/<feature>-"); do
    branch_name=$(echo "$branch" | sed 's/.*\///g')
    echo "  $branch_name: $(git log -1 --oneline "$branch")"
done

echo "\n=== VERIFICATION COMMANDS ==="
echo "User can verify complete state with:"
echo "# Check assumption testing results"
echo "ls -la docs/assumptions/<feature>/"
echo "# Check updated plan"
echo "cat requests/<feature>/assumptions/updated-plan.md"
echo "# Check production branches"
echo "git branch -r | grep 'feature/<feature>-'"
echo "# Check workplan with assumption insights"
echo "ls -la docs/workplans/<feature>/"
```

#### 4. Prototype-First Context Preservation (REQUIRED)
```markdown
## PROTOTYPE-FIRST CONTEXT PRESERVATION CHECKLIST

### For User Reference
- [ ] **Assumption Testing Results Documented**: All testing outcomes and evidence provided
- [ ] **Plan Changes Documented**: How plan was adjusted based on assumption testing
- [ ] **Production Worktree Locations Listed**: Each production worktree path and status
- [ ] **Feature Branches Listed**: Every production branch with validated implementation
- [ ] **Verification Commands Provided**: User can independently verify all results
- [ ] **Next Steps Defined**: Clear guidance on integration of validated implementation

### For Future Implementation Sessions
- [ ] **Validated Approach Documented**: Technical approach proven by assumption testing
- [ ] **Assumption Testing Insights Available**: Complete testing results and lessons learned
- [ ] **Plan Evolution Traced**: How original plan evolved based on evidence
- [ ] **Production Implementation Context**: Why specific implementation choices were made
- [ ] **Integration Strategy Based on Testing**: How components should integrate
- [ ] **Risk Mitigation Validated**: How assumption testing reduced project risks

### Critical Archives for Handoff
**Must be accessible for future sessions:**
- `docs/assumptions/<feature>/` - Complete assumption testing results and evidence
- `requests/<feature>/assumptions/updated-plan.md` - Plan adjusted based on testing
- `docs/workplans/<feature>/` - Updated workplan with assumption testing insights
- Production worktrees in `~/work/worktrees/<feature>/final/` - Validated implementations
- Feature branches - Production code based on assumption-tested approach
- `docs/prototypes/<feature>/` - Archived prototype lessons (if applicable)
```

**⚠️ CRITICAL**: Assumption testing phase must discover working approaches for all critical assumptions before production implementation begins. Plan must be updated to reflect validated approach, not original assumptions. Future agents must understand both what was tested AND what was learned.
```