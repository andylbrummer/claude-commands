# Subtasks Completion & Retrospective
## Large Feature Completion with Cross-Task Learning Capture

## Prerequisites Check

### MANDATORY: Before Completing Subtasks
```markdown
## Integration Review Verification ✅
- [ ] **Integration Review Complete**: All subtasks have been through integration review using [subtasks-review.md](subtasks-review.md)
- [ ] **Review Approval**: Cross-task integration review has been completed and approved with no critical issues
- [ ] **Quality Gates Passed**: All feature-level quality gates and standards have been met
- [ ] **Integration Issues Resolved**: All identified cross-task integration issues have been resolved
- [ ] **Stakeholder Sign-off**: Required stakeholder approvals have been obtained for the complete feature

## Feature-Level Final Verification
- [ ] **End-to-End Testing Complete**: Full feature workflows tested and verified across all components
- [ ] **Performance Benchmarked**: Performance requirements validated across the entire feature
- [ ] **Security Audit Complete**: Security review completed for the full feature surface area
- [ ] **Cross-Service Compatibility**: All cross-service integration points verified and documented
- [ ] **Documentation Complete**: Feature-level documentation updated and comprehensive

## Production Readiness (Large Features)
- [ ] **Monitoring Configured**: Comprehensive monitoring and alerting set up for the complete feature
- [ ] **Rollback Plans Ready**: Feature-level rollback procedures documented and tested
- [ ] **Deployment Strategy**: Deployment approach documented with risk mitigation
- [ ] **Knowledge Transfer Complete**: Knowledge transfer completed to all relevant teams
- [ ] **Archive Structure Ready**: Complete feature archive structure prepared with all subtask artifacts

**⚠️ STOP**: Do not proceed with completion until EVERYTHING has been properly reviewed, integrated, and approved at the feature level
```

## Quick Completion (Simple Features)

### 5-Minute Feature Completion Check:
```markdown
## Feature Complete ✅
- [ ] All subtasks completed and integrated
- [ ] Integration tests passing across all components
- [ ] Feature flags working (if replacing functionality)
- [ ] Context applied consistently across tasks
- [ ] No critical integration issues
- [ ] Performance acceptable for project phase

## Quick Learning Capture
**What worked well**: [Cross-task coordination highlights]
**Context effectiveness**: [Which context sources were most valuable]
**Integration challenges**: [Key integration lessons learned]
**Subagent coordination**: [What worked/didn't work in parallel development]
```

**Simple feature complete** → Archive feature folder and update master context knowledge

---

## Feature Completion Verification (Complex Features)

### Project Phase-Scaled Completion Requirements

#### For PROTOTYPE Phase Features
```markdown
## Core Feature Deliverables ✅
- [ ] All original requirements implemented across subtasks
- [ ] Cross-task integration verified and working
- [ ] Basic end-to-end user workflow functional
- [ ] Context consistently applied across all tasks
- [ ] Performance acceptable for prototype usage

## Context Quality Assessment ✅
- [ ] Context package was comprehensive and usable
- [ ] Similar patterns successfully identified and applied
- [ ] External sources provided useful guidance
- [ ] Subagents had sufficient context to work independently
- [ ] Context sharing across worktrees worked effectively
```

#### For MVP/BETA Phase Features
```markdown
## Production Readiness ✅
- [ ] User Problem Resolution verified across entire feature
- [ ] End-to-end user workflows tested and optimized
- [ ] Performance measured and meets targets across all components
- [ ] Feature flags implemented for safe replacement (if applicable)
- [ ] Cross-service integration properly tested
- [ ] Error handling comprehensive across all subtasks

## Context Application Validation ✅
- [ ] All subagents successfully applied context patterns
- [ ] External standards consistently implemented across tasks
- [ ] Similar implementations properly referenced and adapted
- [ ] Architecture decisions followed throughout feature
- [ ] Integration patterns from context successfully applied
```

#### For PRODUCTION Phase Features  
```markdown
## Full Production Validation ✅
- [ ] Security scan passed for entire feature
- [ ] Performance tested under realistic load across all components
- [ ] Comprehensive monitoring/alerting configured
- [ ] Cross-team coordination completed
- [ ] Business metrics defined and trackable
- [ ] Disaster recovery procedures updated

## Advanced Context Validation ✅
- [ ] Context-driven development proved effective at scale
- [ ] External standards compliance verified across all tasks
- [ ] Architecture integrity maintained throughout implementation
- [ ] Cross-task pattern consistency achieved
- [ ] Context package completeness validated by results
```

### Feature Flag & Replacement Safety Verification

#### Replacement Implementation Validation
```markdown
## MANDATORY: Feature Flag Implementation Complete ✅
- [ ] Feature flag controls entire feature (all subtasks respect flag)
- [ ] Default state preserves existing behavior
- [ ] Flag can be toggled without deployment
- [ ] Monitoring tracks both old and new feature usage/performance
- [ ] All subagents implemented flag checking consistently

## MANDATORY: Parallel Operation Verification ✅
- [ ] Both old and new feature versions operational simultaneously
- [ ] Performance comparison data collected across all components
- [ ] User feedback mechanisms in place for new feature
- [ ] Error rates compared between old and new implementations
- [ ] Cross-task integration works in both flag states

## MANDATORY: Safe Removal Preparation ✅
- [ ] New feature proven superior in ALL metrics across ALL tasks
- [ ] Extended parallel operation period planned (minimum 1 week production)
- [ ] Stakeholder sign-off process defined
- [ ] Old feature removal planned as separate, reviewable changes
- [ ] **RULE: Never remove old feature in same change as new feature**
```

## Context & Process Retrospective

### Context Application Effectiveness Analysis

#### Context Package Quality Assessment
```markdown
## Context Package Evaluation

### Codebase Analysis Effectiveness
**Score**: [X/5]
- **Pattern Discovery**: [How well existing patterns were identified]
- **File Dependency Mapping**: [Accuracy of dependency analysis]
- **Integration Point Identification**: [Completeness of integration analysis]
- **Similar Implementation Value**: [How useful similar code references were]

### External Context Value
**Score**: [X/5]  
- **Documentation Sources**: [Relevance and accuracy of external docs]
- **Standards Application**: [Effectiveness of applying external standards]
- **Reference Implementations**: [Value of external code examples]
- **Version Compatibility**: [Accuracy of dependency version analysis]

### Context Distribution Effectiveness
**Score**: [X/5]
- **Worktree Context Sharing**: [How well context reached all subagents]
- **Context Accessibility**: [Ease of accessing context during development]
- **Context Updates**: [How well context evolved during development]
- **Cross-Task Consistency**: [How well context enabled consistent implementation]
```

#### Subagent Context Application Analysis
```markdown
## Per-Task Context Usage Analysis

### Research Tasks
- **Context Sources Most Valuable**: [Which context elements research tasks used most]
- **Context Gaps Discovered**: [What context was missing for research]
- **Context Updates Contributed**: [How research enhanced context package]

### Development Tasks  
- **Pattern Application Success**: [How well development tasks followed context patterns]
- **External Standards Compliance**: [How effectively standards were applied]
- **Similar Code References**: [How useful existing code references were]
- **Context-Driven Decisions**: [Decisions influenced by context vs created new]

### Integration Tasks
- **Cross-Task Context Consistency**: [How well context enabled integration]
- **Architecture Adherence**: [How well architecture decisions were followed]
- **Integration Pattern Success**: [Effectiveness of context integration guidance]
```

### Subagent Coordination Effectiveness

#### Parallel Development Analysis
```markdown
## Parallel Development Assessment

### Worktree Management Effectiveness
**Score**: [X/5]
- **Setup Efficiency**: [How quickly worktrees were set up]
- **Context Distribution**: [How well context reached all worktrees]
- **Git Coordination**: [Effectiveness of git worktree coordination]
- **Merge Conflicts**: [Frequency and severity of merge conflicts]

### Task Independence Achievement
**Score**: [X/5]
- **Dependency Minimization**: [How well tasks avoided dependencies]
- **Parallel Execution**: [How many tasks could run truly in parallel]
- **Interface Stability**: [How stable cross-task interfaces were]
- **Context Sufficiency**: [How well context enabled independent work]

### Coordination Overhead Analysis
- **Communication Frequency**: [How often subagents needed to coordinate]
- **Context Updates**: [How often context package needed updates]
- **Integration Complexity**: [Difficulty of integrating parallel work]
- **Task Master Interventions**: [How often Task Master needed to intervene]
```

### Process Improvements Discovery

#### Master Planning Process Improvements
```markdown
## Planning Process Lessons

### Context Capture Improvements
- **Missing Context Types**: [Types of context that should be captured but weren't]
- **Context Depth Issues**: [Areas where context was too shallow/deep]
- **Source Discovery**: [Better ways to find relevant context sources]
- **Pattern Identification**: [Improved methods for identifying useful patterns]

### Task Decomposition Improvements  
- **File Scope Sizing**: [Optimal file scope sizes discovered]
- **Risk Assessment**: [Better ways to assess and order task risks]
- **Dependency Management**: [Improved dependency identification and management]
- **Persona Assignment**: [More effective persona-to-task matching]

### Context Package Structure Improvements
- **Information Architecture**: [Better ways to organize context information]
- **Access Patterns**: [How subagents actually used context vs planned]
- **Update Mechanisms**: [Better ways to evolve context during development]
- **Distribution Methods**: [More effective context sharing approaches]
```

#### Execution Process Improvements
```markdown
## Execution Process Lessons

### Subagent Coordination Improvements
- **Status Reporting**: [Better ways for subagents to report status]
- **Conflict Resolution**: [Improved methods for resolving conflicts]
- **Context Synchronization**: [Better ways to keep context current]
- **Integration Points**: [Improved coordination at integration boundaries]

### Worktree Management Improvements
- **Setup Automation**: [Ways to automate worktree setup]
- **Context Linking**: [Better methods for sharing context across worktrees]
- **Cleanup Processes**: [Improved worktree cleanup and archival]
- **Performance Optimization**: [Ways to optimize worktree performance]

### Quality Assurance Improvements
- **Context Validation**: [Better ways to verify context application]
- **Integration Testing**: [Improved integration testing approaches]
- **Cross-Task Consistency**: [Better methods for ensuring consistency]
- **Performance Monitoring**: [Improved performance tracking across tasks]
```

## Knowledge Capture & Future Applications

### Pattern Documentation Updates
```markdown
## New Patterns Discovered

### Implementation Patterns
- **Pattern Name**: [Description] - [When to use] - [Files demonstrating pattern]
- **Integration Pattern**: [Description] - [Cross-service applicability] - [Context requirements]
- **Context Pattern**: [Description] - [Context types needed] - [Distribution method]

### Anti-Patterns Identified
- **Anti-Pattern**: [What doesn't work] - [Why it fails] - [Better alternative]
- **Context Anti-Pattern**: [Context approach that failed] - [Root cause] - [Improved approach]
- **Coordination Anti-Pattern**: [Coordination approach that failed] - [Impact] - [Better method]

### Reusable Context Templates
- **Feature Type A Context Template**: [Context requirements for similar features]
- **Integration Context Template**: [Standard context for cross-service features]
- **Replacement Context Template**: [Context needed for feature replacements]
```

### Methodology Evolution
```markdown
## Methodology Improvements for Future Features

### Context Capture Enhancements
- **Automated Discovery**: [Tools/scripts for better context discovery]
- **Context Templates**: [Standard templates for common feature types]
- **Source Validation**: [Automated validation of context sources]
- **Pattern Mining**: [Better methods for discovering reusable patterns]

### Subagent Coordination Enhancements
- **Communication Protocols**: [Improved subagent communication methods]
- **Status Dashboards**: [Better visibility into parallel development]
- **Conflict Prevention**: [Proactive conflict avoidance strategies]
- **Integration Automation**: [Automated integration verification]

### Quality Assurance Enhancements  
- **Context Compliance**: [Automated verification of context application]
- **Integration Testing**: [Improved cross-task integration testing]
- **Performance Regression**: [Better performance regression detection]
- **Documentation Generation**: [Automated documentation from context and implementation]
```

## MANDATORY Subtasks Completion Requirements

### ⚠️ CRITICAL: Large Feature Completion Phase Deliverables
```markdown
## 1. Updated User Documentation ✅
- [ ] **Feature Documentation Updated**: All user-facing documentation reflects the complete feature
- [ ] **Integration Documentation**: How the feature integrates with existing systems
- [ ] **User Workflow Documentation**: End-to-end user workflows updated
- [ ] **API Documentation**: All new APIs documented with examples
- [ ] **Configuration Documentation**: New configuration options documented
- [ ] **Migration Guides**: Any required migration steps documented
- [ ] **Troubleshooting Guides**: Common issues and solutions documented

## 2. Simple Feature Review Command/Link ✅
- [ ] **Feature Demo Command**: Single command to demonstrate the complete feature
- [ ] **Integration Test Command**: Command to verify all subtasks work together
- [ ] **Feature Toggle Command**: Command to enable/disable the feature (if applicable)
- [ ] **Health Check Command**: Command to verify feature is working correctly
- [ ] **Performance Test Command**: Command to verify performance characteristics

## 3. Archive Location Documentation ✅
- [ ] **Main Archive Path**: Exact location of complete feature archive
- [ ] **Subtask Archives**: Locations of individual subtask archives
- [ ] **Context Archive**: Location of context package and research
- [ ] **Worktree Archives**: Location of all worktree bundles
- [ ] **Integration Archive**: Location of integration test results and reports
- [ ] **Archive Access Guide**: Clear instructions for accessing archived materials

## 4. Cross-Task Lessons Learned Extraction ✅
- [ ] **Integration Lessons**: What worked/didn't work for cross-task integration
- [ ] **Context Effectiveness**: How well context packages enabled independent work
- [ ] **Parallel Development**: Lessons from parallel subtask development
- [ ] **Coordination Overhead**: Analysis of coordination costs and benefits
- [ ] **Quality Consistency**: How well quality was maintained across subtasks
- [ ] **Performance Impact**: Cumulative performance impact of all subtasks
- [ ] **Technical Debt Assessment**: Technical debt introduced/resolved across feature

## 5. Archive vs Active Work Warnings ✅
- [ ] **Feature Archive Status**: Clear marking of complete feature as archived
- [ ] **Subtask Archive Status**: All subtask plans marked as historical reference
- [ ] **Context Archive Status**: Context packages marked as reference material
- [ ] **Integration Plans Status**: Integration plans marked as completed/archived
- [ ] **Future Work Guidance**: Clear guidance for future agents on using archives
```

### Critical Large Feature Completion Template
```markdown
# LARGE FEATURE COMPLETION DELIVERABLES

## 1. User Documentation Updates

### Feature-Level Documentation Changes:
- **Feature Overview**: [path/to/feature-overview] - **Status**: Updated
- **User Workflows**: [path/to/workflow-docs] - **Status**: Updated  
- **API Documentation**: [path/to/api-docs] - **Status**: Updated
- **Configuration Guide**: [path/to/config-docs] - **Status**: Updated
- **Migration Guide**: [path/to/migration-guide] - **Status**: Created/Updated
- **Troubleshooting**: [path/to/troubleshooting] - **Status**: Updated

### Documentation Verification:
- [ ] All feature workflows tested end-to-end
- [ ] All API examples verified working
- [ ] All configuration options tested
- [ ] Migration guide tested on realistic data
- [ ] Troubleshooting guide covers real issues encountered

## 2. Feature Review Information

### Complete Feature Demo:
```bash
# Command to demonstrate the complete feature:
[exact command sequence to show full feature working]
```

### Feature Components:
- **Main Entry Points**: `/path/to/main/feature/entry/points`
- **Core Services**: `/path/to/core/services`
- **User Interfaces**: `/path/to/ui/components`
- **Configuration Files**: `/path/to/config/files`
- **Integration Points**: `/path/to/integration/points`
- **Test Suites**: `/path/to/test/suites`

### Feature Verification Commands:
```bash
# Health check command:
[command to verify feature health]

# Integration test command:
[command to run integration tests]

# Performance test command:
[command to verify performance]

# Feature toggle commands:
[commands to enable/disable feature if applicable]
```

## 3. Complete Archive Documentation

### Main Feature Archive:
- **Path**: `/archive/features/YYYY-MM-DD-feature-name/`
- **Created**: [date]
- **Archived By**: [task master name/role]

### Archive Structure:
```
/archive/features/YYYY-MM-DD-feature-name/
├── README.md                    # Archive overview with warnings
├── ARCHIVE_WARNING.md           # Critical warnings for future agents
├── context/                     # Context packages used
│   ├── codebase-analysis/
│   ├── external-docs/
│   └── similar-implementations/
├── tasks/                       # All subtask documentation
│   ├── task-01-research/
│   ├── task-02-backend/
│   ├── task-03-frontend/
│   └── task-04-integration/
├── learnings/                   # Retrospectives and lessons
│   ├── feature-retrospective.md
│   ├── integration-review-report.md
│   └── cross-task-lessons.md
└── worktrees/                   # Git bundles of all worktrees
    ├── research-worktree.bundle
    ├── backend-worktree.bundle
    └── frontend-worktree.bundle
```

### Archive Access Commands:
```bash
# To explore the complete archive:
ls -la /archive/features/YYYY-MM-DD-feature-name/
cat /archive/features/YYYY-MM-DD-feature-name/README.md

# To review specific subtask:
cat /archive/features/YYYY-MM-DD-feature-name/tasks/task-01-research/retrospective.md

# To restore a worktree for reference:
git clone /archive/features/YYYY-MM-DD-feature-name/worktrees/backend-worktree.bundle temp-reference
```

## 4. Cross-Task Lessons Learned

### Integration Effectiveness:
- **Cross-Task Coordination**: [How well subtasks coordinated]
- **Interface Stability**: [How stable cross-task interfaces were]
- **Integration Complexity**: [Actual vs expected integration effort]
- **Conflict Resolution**: [How conflicts between subtasks were resolved]

### Context Package Effectiveness:
- **Context Completeness**: [How well context enabled independent work]
- **Context Usage**: [How effectively subtasks used provided context]
- **Context Evolution**: [How context was updated during development]
- **Context Distribution**: [How well context reached all subtasks]

### Parallel Development Analysis:
- **True Parallelism Achieved**: [What percentage of work was truly parallel]
- **Coordination Overhead**: [Time spent on coordination vs development]
- **Worktree Management**: [Effectiveness of worktree-based approach]
- **Merge Complexity**: [Difficulty of integrating parallel work]

### Quality Consistency:
- **Standard Application**: [How consistently standards were applied]
- **Code Quality Variance**: [Quality differences between subtasks]
- **Testing Consistency**: [How consistently testing approaches were applied]
- **Documentation Quality**: [Consistency of documentation across subtasks]

### Performance Impact Assessment:
- **Individual Performance**: [Performance of each subtask]
- **Cumulative Impact**: [Total performance impact of complete feature]
- **Performance Regression**: [Any performance regressions introduced]
- **Optimization Opportunities**: [Performance improvements identified]

### Time/Effort Analysis:
- **Planned vs Actual**: [Comparison of estimated vs actual effort]
- **Parallel Efficiency**: [Efficiency gains from parallel development]
- **Coordination Costs**: [Time spent on coordination and integration]
- **Context Preparation ROI**: [Return on investment for context preparation]

## 5. ⚠️ CRITICAL: Feature Archive Status Warnings

### For Future Agents:
```
🔴 WARNING: COMPLETE FEATURE ARCHIVED - REFERENCE ONLY

- This feature is COMPLETE and all subtasks are FINISHED
- Archived plans, todos, and context are HISTORICAL REFERENCE ONLY
- DO NOT execute archived subtask plans
- DO NOT reopen archived subtask todos
- Use archives for LEARNING about successful feature development
- Always create NEW plans for NEW features
```

### Feature Archive Usage Guidelines:
✅ **DO**: Study feature architecture and integration patterns
✅ **DO**: Learn from cross-task coordination approaches
✅ **DO**: Reference context preparation methods
✅ **DO**: Understand parallel development lessons
✅ **DO**: Review integration testing strategies
✅ **DO**: Study performance optimization approaches

❌ **DON'T**: Execute any archived subtask plans
❌ **DON'T**: Reopen any archived subtask todos
❌ **DON'T**: Copy archived plans without complete update for current context
❌ **DON'T**: Assume archived technical decisions still apply
❌ **DON'T**: Treat any archived work as active
❌ **DON'T**: Modify archived materials (they are historical record)

### Archive Header Template for All Subtasks:
```markdown
# 🗄️ ARCHIVED SUBTASK - REFERENCE ONLY

**Feature**: [feature-name]
**Subtask**: [subtask-name] 
**Completed**: [date]
**Status**: ✅ COMPLETED - 📚 ARCHIVED

## ⚠️ SUBTASK ARCHIVE WARNING

🔴 **This subtask is COMPLETE and part of a FINISHED feature**
- DO NOT execute plans or todos from this archive
- This subtask was successfully integrated into complete feature
- Use for LEARNING how this type of subtask was approached
- Create NEW subtask plans for NEW features

## Integration Status
- **Integration**: ✅ Successfully integrated with [list other subtasks]
- **Feature Status**: ✅ Complete feature deployed and working
- **Quality Gates**: ✅ All quality gates passed

## Learning Value
✅ **Learn from**: retrospective.md, technical-notes.md, integration-approach.md
✅ **Reference**: Code patterns and architecture decisions
✅ **Understand**: How this subtask fit into larger feature

❌ **Don't**: Execute todos, copy plans, assume decisions still apply
```
```

## Feature Archival & Knowledge Transfer

### Archive Structure Creation
```bash
# Archive completed feature with full context and learnings
FEATURE_NAME="<feature-name>"
ARCHIVE_DATE=$(date +%Y%m%d)
ARCHIVE_PATH="/archive/features/${ARCHIVE_DATE}-${FEATURE_NAME}"

# Create comprehensive archive with warnings
mkdir -p "$ARCHIVE_PATH"/{context,tasks,learnings,worktrees}

# Archive context package
cp -r "requests/$FEATURE_NAME/context/" "$ARCHIVE_PATH/context/"

# Archive all task files with warnings
cp -r "requests/$FEATURE_NAME/tasks/" "$ARCHIVE_PATH/tasks/"

# Add archive warnings to all task files
find "$ARCHIVE_PATH/tasks/" -name "*.md" -exec echo "\n\n# 🔴 ARCHIVE WARNING\nThis file is part of COMPLETED feature. DO NOT execute plans or reopen todos. Use for learning only." >> {} \;

# Archive worktree states
for worktree in ~/work/worktrees/$FEATURE_NAME/*/; do
    worktree_name=$(basename "$worktree")
    git -C "$worktree" bundle create "$ARCHIVE_PATH/worktrees/${worktree_name}.bundle" --all
done

# Archive learnings and retrospective
cp subtasks-retrospective.md "$ARCHIVE_PATH/learnings/"
cp integration-review-report.md "$ARCHIVE_PATH/learnings/"

# Create main archive README with warnings
cat > "$ARCHIVE_PATH/README.md" << EOF
# 🗄️ ARCHIVED FEATURE - REFERENCE ONLY

**Feature**: $FEATURE_NAME
**Completed**: $ARCHIVE_DATE
**Status**: ✅ COMPLETED - 📚 ARCHIVED

## ⚠️ FEATURE ARCHIVE WARNING

🔴 **This feature is COMPLETE with all subtasks FINISHED**
- DO NOT execute any plans or todos from this archive
- All subtasks were successfully completed and integrated
- Feature is deployed and working in production
- Use this archive for LEARNING and REFERENCE only

## Archive Purpose
✅ **Learn from**: Cross-task integration approaches
✅ **Reference**: Feature architecture and patterns
✅ **Understand**: Parallel development coordination
✅ **Study**: Context-driven development effectiveness

❌ **Don't**: Execute subtask plans, reopen todos, copy without updates

## Quick Access
- Context packages: ./context/
- All subtasks: ./tasks/
- Lessons learned: ./learnings/
- Worktree bundles: ./worktrees/

## Feature Demo
\`\`\`bash
# To see the completed feature in action:
[demo command from completion template]
\`\`\`
EOF

# Create critical warning file
cat > "$ARCHIVE_PATH/ARCHIVE_WARNING.md" << EOF
# 🔴 CRITICAL WARNING FOR FUTURE AGENTS

## This Directory Contains a COMPLETED, ARCHIVED Feature

**Status**: ✅ COMPLETE - All subtasks finished on $ARCHIVE_DATE
**Archive Purpose**: Reference and learning only

### This Archive Contains:
- Complete feature with all subtasks integrated
- Context packages that enabled parallel development
- All subtask plans (COMPLETED - do not execute)
- Integration approach and results
- Cross-task coordination lessons learned
- Worktree bundles for code reference

### What This Archive Is For:
✅ Learning how to coordinate multiple subtasks
✅ Understanding context-driven development
✅ Studying parallel development approaches
✅ Referencing integration patterns
✅ Understanding feature architecture

### What This Archive Is NOT For:
❌ Executing any subtask plans (they are COMPLETE)
❌ Reopening any subtask todos (they are FINISHED)
❌ Copying plans without updating for current context
❌ Assuming technical decisions still apply to new work
❌ Treating any part of this as active work

### For New Similar Features:
1. Review learnings/feature-retrospective.md for lessons learned
2. Study context/ to understand context preparation approaches  
3. Review tasks/ to understand subtask decomposition
4. Study integration approach in learnings/integration-review-report.md
5. Create NEW feature plans informed by archived learnings
6. DO NOT copy/execute any archived subtask plans

### Archive Maintenance:
This archive should remain unchanged as historical record of successful feature development.
EOF

echo "Feature archived to: $ARCHIVE_PATH"
echo "⚠️ Archive includes completion warnings for future agents"
```

### Knowledge Transfer Documentation
```markdown
## Knowledge Transfer Package

### For Future Similar Features
- **Context Requirements**: [Specific context needed for similar features]
- **Pattern Applications**: [Patterns that worked well for this type of feature]
- **Common Pitfalls**: [Issues to avoid in similar features]
- **Resource Requirements**: [Time/effort estimates for similar features]
- **Integration Complexity**: [Expected integration effort for similar features]
- **Parallel Development Suitability**: [When/how to use parallel subtask approach]

### For Task Masters
- **Context Discovery**: [Effective methods used for context discovery]
- **Task Decomposition**: [Successful decomposition strategies]
- **Coordination Approaches**: [Effective subagent coordination methods]
- **Quality Gates**: [Quality checkpoints that caught issues]
- **Integration Planning**: [How to plan for subtask integration]
- **Worktree Management**: [Best practices for worktree coordination]

### For Subagents
- **Context Usage**: [How to effectively use context packages]
- **Pattern Application**: [Guidelines for applying context patterns]
- **Integration Points**: [How to coordinate with other subagents]
- **Quality Standards**: [Quality expectations and verification methods]
- **Communication Protocols**: [How to communicate with other subtasks]
- **Independence Best Practices**: [How to work independently while maintaining integration]

### For Process Improvement
- **Methodology Updates**: [Specific improvements to methodology documents]
- **Tool Enhancements**: [Tool improvements that would help future features]
- **Template Updates**: [Template improvements based on experience]
- **Training Needs**: [Skills/knowledge gaps identified]
- **Context Package Templates**: [Reusable context templates for feature types]
- **Integration Testing Improvements**: [Better approaches for integration testing]

### ⚠️ CRITICAL: Future Agent Archive Guidance
- **Complete Archive Location**: [Exact path to feature archive]
- **Most Valuable Learning Resources**: [Which archived docs provide most value for learning]
- **Reusable Patterns**: [Patterns from this feature that can be adapted for new work]
- **Anti-Patterns**: [Approaches that didn't work well and should be avoided]
- **Context Dependencies**: [What context knowledge is needed to understand this feature]
- **Integration Lessons**: [Key lessons about integrating multiple subtasks]

### Archive Usage Warning for Task Masters
```
🔴 IMPORTANT FOR FUTURE TASK MASTERS:

This completed feature has been archived to: [archive-path]

For planning similar features:
✅ Study the context preparation approach in context/
✅ Review subtask decomposition strategy in tasks/
✅ Learn from integration approach in learnings/
✅ Understand coordination overhead and benefits
✅ Review parallel development effectiveness

❌ DO NOT copy subtask plans without complete updates
❌ DO NOT assume context packages still apply to new features
❌ DO NOT reuse integration plans without validation
❌ DO NOT skip context preparation based on archived examples

For new features: Create fresh plans informed by archived learnings
```

### Archive Usage Warning for Subagents
```
🔴 IMPORTANT FOR FUTURE SUBAGENTS:

This archived feature shows successful subtask coordination:
✅ Study how context packages enabled independent work
✅ Learn from integration point management
✅ Understand quality consistency approaches
✅ Review communication and coordination patterns

❌ DO NOT execute any archived subtask plans
❌ DO NOT reopen any archived subtask todos
❌ DO NOT assume archived decisions apply to new work
❌ DO NOT copy code without understanding current context

For new subtasks: Use archived learnings to inform new approaches
```
```

### Final Feature Certification
```markdown
# FEATURE COMPLETION CERTIFICATION

## Feature: [Feature Name]
## Completion Date: [Date]
## Task Master: [Name/Role]

### Verification Statements
- ✅ All subtasks completed with context consistently applied
- ✅ Cross-task integration verified and working
- ✅ Feature flags implemented and tested (if applicable)
- ✅ Context-driven development proved effective
- ✅ Quality standards met across all components
- ✅ Knowledge captured and transferred for future features

### Context Application Success
- ✅ Context package was comprehensive and effective
- ✅ Subagents successfully worked independently using context
- ✅ External standards properly applied across all tasks
- ✅ Similar patterns successfully identified and reused
- ✅ Architecture integrity maintained throughout development

### Process Innovation Success
- ✅ Parallel development with minimal conflicts achieved
- ✅ Worktree coordination proved effective
- ✅ Context distribution enabled independent work
- ✅ Integration review caught and resolved issues
- ✅ Methodology improvements identified and documented

### Final Status: COMPLETE ✅

**Task Master Signature**: [Name, Role, Date]
**Integration Review Approval**: [Reviewer Name, Date]
**Business Stakeholder Approval**: [Stakeholder Name, Date] (if applicable)

## Feature Lifecycle Complete
**All Phases**: Planning → Execution → Review → Completion → **Learning Applied** ✅
```