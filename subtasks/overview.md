# Subtasks Management System Overview
## Large Feature Development with Context-Aware Subagent Coordination

## The Four-Phase Subtasks Methodology

### 📋 [subtasks-plan.md](subtasks-plan.md) - Master Planning Phase
**When to use**: Large features (>10 files, >16 hours, multiple domains)
**Time investment**: 1-3 hours for comprehensive context capture and task decomposition
**Output**: Context package + individual task files ready for parallel execution
**Key Features**: Deep codebase analysis, external source documentation, worktree setup

### ⚡ [subtasks-execute.md](subtasks-execute.md) - Parallel Execution Phase  
**When to use**: Coordinating multiple subagents in parallel worktrees
**Focus**: Context preservation, subagent coordination, cross-task communication
**Output**: Completed parallel tasks with context consistently applied
**Key Features**: Worktree management, context distribution, progress monitoring

### ✅ [subtasks-review.md](subtasks-review.md) - Integration Review Phase
**When to use**: All completed subtask collections requiring integration
**Time investment**: 30 minutes to 2 hours depending on complexity
**Output**: Integration verification, cross-task consistency validation
**Key Features**: Context application verification, integration testing, issue resolution

### 🎯 [subtasks-complete.md](subtasks-complete.md) - Feature Completion Phase
**When to use**: All integrated large features
**Time investment**: 1-3 hours for comprehensive retrospective and knowledge capture
**Output**: Feature certification, process improvement insights, knowledge transfer
**Key Features**: Context effectiveness analysis, methodology evolution, pattern documentation

## Quick Decision Guide

### "This is a small feature"
→ **Don't use subtasks system** - Go to [tasks-plan.md](tasks-plan.md) instead

### "Medium complexity feature" (4-10 files, 4-16 hours)
→ Use [tasks-plan.md](tasks-plan.md) with careful breakdown instead of subtasks

### "Large feature with multiple domains"
→ Start with [subtasks-plan.md](subtasks-plan.md) (Master planning)

### "Multi-service integration"
→ **MANDATORY**: [subtasks-plan.md](subtasks-plan.md) (Full methodology + cross-service coordination)

### "Replacing existing large system"
→ **MANDATORY**: [subtasks-plan.md](subtasks-plan.md) (Full methodology + feature flags across all tasks)

## Key Safety Rules for Large Features

### 🚫 NEVER REMOVE old functionality without:
1. Feature flags implemented **across all subtasks**
2. Extended parallel operation (min 1 week production)
3. **Every subagent** respecting feature flag state
4. Cross-task integration tested in both flag states
5. Separate removal commits **for each component**

### ⚠️ CRITICAL COORDINATION GOTCHAS (Prevent Project Disasters):

#### File Edit Conflicts (High Risk)
- **Interface Files**: Multiple tasks editing `*.types.ts` simultaneously
  - **Prevention**: Interface versioning strategy + sequential editing protocol
  - **Detection**: Check for duplicate interface definitions across worktrees
- **Configuration Files**: `package.json`, `*.config.*` merge conflicts
  - **Prevention**: Namespace separation + structured merging
  - **Detection**: Monitor config file changes across all tasks

#### Context Synchronization Issues (Medium Risk)  
- **Stale Context**: External dependencies change during long development
  - **Prevention**: Context freshness checks every 24h
  - **Detection**: URL validation + pattern verification scripts
- **Pattern Drift**: Subagents applying inconsistent patterns
  - **Prevention**: Pattern consistency validation + cross-task reviews
  - **Detection**: Pattern application audits across all implementations

#### Resource Contention (Medium Risk)
- **Port/Database Conflicts**: Multiple tasks using same test resources
  - **Prevention**: Resource allocation per task + validation scripts
  - **Detection**: Port scanning + database access verification
- **Build Tool Conflicts**: Package lock file conflicts during parallel installs
  - **Prevention**: Isolated dependency management per worktree
  - **Detection**: Lock file conflict monitoring

### 📚 ALWAYS CAPTURE COMPREHENSIVE CONTEXT (Mandatory):
1. **Master Codebase Analysis**: Deep file dependency mapping, pattern discovery
2. **Comprehensive External Research**: Documentation URLs, standards, examples with specific relevance
3. **Architecture Integration**: Service boundaries, data flow, integration patterns
4. **Context Package Distribution**: Shared context accessible to all subagents
5. **Cross-Task Pattern Consistency**: Same patterns applied across all parallel tasks

### 🔄 PARALLEL DEVELOPMENT COORDINATION:
1. **Worktree Isolation**: Each subagent works in dedicated worktree
2. **Context Synchronization**: Shared context package across all worktrees
3. **Integration Boundaries**: Clear interfaces between parallel tasks
4. **Cross-Task Communication**: Structured coordination between subagents
5. **Master Orchestration**: Task Master coordinates and monitors all parallel work

### 📊 Scale methodology by feature complexity:
- **Large Prototype**: Fast parallel development, context-informed but speed-focused
- **Large MVP/Beta**: User validation, comprehensive context application, integration testing
- **Large Production**: Full security, monitoring, cross-team coordination, complete context

## Context-Aware Development Flow

```
Master Planning → Parallel Execution → Integration Review → Feature Completion
      ↓                   ↓                    ↓                   ↓
  Context Package → Subagent Tasks → Integration Tests → Knowledge Capture
      ↓                   ↓                    ↓                   ↓
Archive with complete context trail and cross-task learnings
```

## Worktree Structure

```
~/work/worktrees/<feature-name>/
├── context-package/          # Shared context for all subagents
│   ├── codebase-analysis.md
│   ├── external-sources.md  
│   ├── architecture-decisions.md
│   └── implementation-patterns.md
├── 01-research-api/          # Research subagent worktree
│   ├── .git -> feature branch
│   ├── context -> ../context-package/
│   └── [research outputs]
├── 02-arch-design/           # Architecture subagent worktree
│   ├── .git -> feature branch
│   ├── context -> ../context-package/
│   └── [design artifacts]
├── 03-dev-backend/           # Backend development worktree
│   ├── .git -> feature branch
│   ├── context -> ../context-package/
│   └── [backend implementation]
├── 03-dev-frontend/          # Frontend development worktree
│   ├── .git -> feature branch
│   ├── context -> ../context-package/
│   └── [frontend implementation]
└── 04-integration/           # Integration testing worktree
    ├── .git -> feature branch
    ├── context -> ../context-package/
    └── [integration tests]
```

## Common Large Feature Workflows

### Large New Feature Development:
1. [subtasks-plan.md](subtasks-plan.md) (Master planning + comprehensive context capture)
   - **Context Requirements**: Full codebase analysis, external research, pattern discovery
2. [subtasks-execute.md](subtasks-execute.md) (Parallel subagent coordination)
   - **Parallel Tasks**: Research, architecture, multiple development streams
3. [subtasks-review.md](subtasks-review.md) (Cross-task integration verification)
   - **Integration Focus**: Context consistency, component integration, pattern compliance
4. [subtasks-complete.md](subtasks-complete.md) (Feature completion + methodology evolution)
   - **Learning Capture**: Context effectiveness, coordination insights, process improvements

### Large System Replacement:
1. [subtasks-plan.md](subtasks-plan.md) (Mandatory comprehensive planning)
   - **Context Requirements**: Current system analysis, replacement strategy, migration planning
2. [subtasks-execute.md](subtasks-execute.md) (Feature flag coordination across all tasks)
   - **Safety Focus**: Parallel operation, flag consistency, rollback capability
3. [subtasks-review.md](subtasks-review.md) (Both system validation)
   - **Dual System Testing**: Old and new system verification, integration compatibility
4. [subtasks-complete.md](subtasks-complete.md) (Safe replacement verification)
   - **Replacement Safety**: Extended parallel operation, performance comparison, removal planning

### Multi-Service Integration:
1. [subtasks-plan.md](subtasks-plan.md) (Cross-service context capture)
   - **Context Requirements**: Service boundary analysis, API contract planning, data flow mapping
2. [subtasks-execute.md](subtasks-execute.md) (Coordinated service development)
   - **Coordination Focus**: API consistency, data compatibility, service communication
3. [subtasks-review.md](subtasks-review.md) (End-to-end integration testing)
   - **Integration Testing**: Cross-service workflows, performance impact, failure scenarios
4. [subtasks-complete.md](subtasks-complete.md) (Service integration certification)
   - **Integration Validation**: Service compatibility, monitoring setup, operational procedures

## Context Package Requirements

### Mandatory Context Elements:
```markdown
## Context Package Contents

### codebase-analysis.md
- **File Dependency Mapping**: Complete mapping of files and their relationships
- **Architecture Patterns**: Current architectural patterns with examples
- **Similar Implementations**: Existing features with similar patterns
- **Integration Points**: Service boundaries and communication patterns

### external-sources.md  
- **Documentation URLs**: All relevant external documentation with specific relevance
- **Standards & Guidelines**: Applicable standards with implementation guidance
- **Reference Implementations**: External code examples with adaptation notes
- **Dependency Analysis**: Library versions, compatibility, and usage patterns

### architecture-decisions.md
- **Design Decisions**: Architectural choices with rationale
- **Trade-offs Analysis**: Alternatives considered and why choices were made
- **Integration Strategy**: How feature integrates with existing system
- **Performance Considerations**: Performance targets and optimization strategies

### implementation-patterns.md
- **Code Patterns**: Specific patterns to follow from existing codebase
- **Naming Conventions**: Consistent naming across all subtasks
- **Error Handling**: Standard error handling patterns
- **Testing Patterns**: Test structure and coverage requirements
```

## Subagent Personas & Coordination

### Task Types & Coordination:
- **Research Tasks** (`01-research-*`) → Research Analyst: Context discovery and validation
- **Architecture Tasks** (`02-arch-*`) → System Architect: Design decisions and integration planning
- **Development Tasks** (`03-dev-*`) → Software Engineers: Parallel implementation streams
- **Testing Tasks** (`04-test-*`) → QA Engineers: Component and integration testing
- **Review Tasks** (`05-review-*`) → Senior Engineers: Quality and integration verification
- **Integration Tasks** (`06-integration-*`) → DevOps Engineers: Service integration and deployment
- **Documentation Tasks** (`07-docs-*`) → Technical Writers: Feature documentation

### Coordination Patterns:
- **Sequential Dependencies**: Research → Architecture → Development → Integration
- **Parallel Development**: Multiple development streams with shared context
- **Cross-Task Communication**: Structured interfaces and integration points
- **Context Synchronization**: Shared context updates across all subagents

### ⚠️ COORDINATION REALITY CHECK:
- **Over-Communication**: Subagents spamming Task Master vs critical issues being ignored
  - **Solution**: Escalation decision trees with clear criteria
- **Integration Timing**: Premature integration attempts when tasks are 50% complete
  - **Solution**: Integration readiness protocol with completion verification
- **Context Staleness**: External dependencies changing during development
  - **Solution**: Context freshness validation + periodic updates

## Integration with Main Task System

### When to Use Subtasks vs Tasks:
```markdown
## Decision Matrix

| Feature Characteristics | Use System |
|------------------------|------------|
| 1-3 files, <4 hours, single domain | [tasks-*](tasks-overview.md) |
| 4-10 files, 4-16 hours, moderate complexity | [tasks-*](tasks-overview.md) with breakdown |
| >10 files, >16 hours, multiple domains | **subtasks-*** |
| Cross-service integration | **subtasks-*** |
| System replacement | **subtasks-*** (mandatory) |
| Multiple parallel work streams needed | **subtasks-*** |
```

### Handoff Points:
- **Simple → Complex**: When task breakdown reveals >10 files or multiple domains
- **Complex → Simple**: When subtasks can be completed as individual tasks
- **Emergency Mode**: Complex features may use simplified task approach with context shortcuts

## Success Metrics for Large Features

### Context Application Success:
- **Pattern Consistency**: Same patterns applied across all parallel tasks
- **Source Utilization**: External sources actively used and referenced
- **Architecture Adherence**: Architecture decisions followed throughout implementation
- **Integration Quality**: Components integrate smoothly with minimal conflicts

### Coordination Effectiveness:
- **Parallel Efficiency**: Multiple subagents working simultaneously with minimal conflicts
- **Context Synchronization**: Shared context enables independent work
- **Integration Speed**: Components integrate quickly due to consistent patterns
- **Quality Consistency**: Uniform quality across all parallel implementations

### Process Innovation:
- **Methodology Evolution**: Process improvements captured and applied
- **Pattern Discovery**: New reusable patterns identified and documented
- **Tool Enhancement**: Tool and process improvements identified
- **Knowledge Transfer**: Effective knowledge transfer to future similar features

This subtasks system scales the proven four-phase methodology to large, complex features requiring parallel development while maintaining the context-aware, practical focus essential for effective software engineering.