# Prototype-First Subagent System Overview
## Assumption-Driven Feature Development with AI Agent Validation

## The AI-Enhanced Four-Phase Methodology

### 📋 [plan.md](plan.md) - Assumption-Driven Planning Phase
**When to use**: Large features with critical assumptions to validate
**Time investment**: 1-3 hours planning + 2-8 hours assumption testing with AI agents
**Output**: Context package + validated approaches + updated task files
**Key Features**: Critical assumption identification, lightweight investigation apps, evidence-based planning

### ⚡ [execute.md](execute.md) - AI Assumption Testing + Production Phase  
**When to use**: Testing critical assumptions before full implementation
**Focus**: AI agent coordination for rapid assumption validation, alternative approach testing
**Output**: Validated technical approaches + production implementation using proven patterns
**Key Features**: Multiple AI prototype testing, evidence-based decisions, plan adjustment

### ✅ [review.md](review.md) - Assumption-Validation Review Phase
**When to use**: All assumption-tested implementations requiring verification
**Time investment**: 30 minutes to 2 hours validating assumption testing results
**Output**: Verification that production matches tested assumptions
**Key Features**: Testing-to-production alignment, assumption validation results, evidence verification

### 🎯 [complete.md](complete.md) - AI Testing Value Assessment Phase
**When to use**: All assumption-tested features for learning capture
**Time investment**: 1-3 hours documenting AI testing value and insights
**Output**: AI testing ROI documentation, process refinement, methodology evolution
**Key Features**: Testing effectiveness analysis, quality improvement measurement, AI agent coordination learnings

## Quick Decision Guide

### "This is a small feature"
→ **Don't use subtasks system** - Go to [tasks-plan.md](tasks-plan.md) instead

### "Medium complexity feature" (4-10 files, 4-16 hours)
→ Use [tasks-plan.md](tasks-plan.md) with careful breakdown instead of subtasks

### "Large feature with critical assumptions"
→ Start with [plan.md](plan.md) (Assumption identification + AI testing planning)

### "Multi-service integration with unknown dependencies"
→ **MANDATORY**: [plan.md](plan.md) (Full methodology + assumption testing for integration points)

### "Replacing existing system with performance/compatibility unknowns"
→ **MANDATORY**: [plan.md](plan.md) (Assumption testing + feature flags validation)

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

## AI-Enhanced Assumption-Driven Development Flow

```
Assumption Planning → AI Testing → Plan Adjustment → Production → Validation
       ↓                ↓             ↓             ↓           ↓
   Context Package → AI Prototypes → Updated Plan → Production → Value Assessment
       ↓                ↓             ↓             ↓           ↓
Archive with assumption testing insights and AI coordination learnings
```

## AI Agent Testing Strategy
**Core Hypothesis**: AI coding speed + multiple prototypes = higher quality than single implementation

### Speed Advantage:
- **5-10x Testing Speed**: AI agents can test multiple approaches in time for 1 manual analysis
- **Parallel Alternative Testing**: Multiple AI agents testing different solutions simultaneously
- **Rapid Failure Discovery**: Quickly identify invalid assumptions before full implementation
- **Evidence-Based Decisions**: Choose approaches based on testing results, not assumptions

## Assumption Testing Worktree Structure

```
~/work/worktrees/<feature-name>/
├── assumptions/              # Assumption testing results
│   ├── critical-assumptions.md
│   ├── testing-results-summary.md
│   └── plan-changes.md
├── context/                  # Enhanced context with testing insights
│   ├── codebase-analysis.md
│   ├── external-sources.md  
│   ├── assumption-testing-results.md    # NEW: Testing evidence
│   ├── architecture-decisions.md        # Updated with validated approaches
│   └── implementation-patterns.md       # Enhanced with tested patterns
├── proto/                    # AI assumption testing worktrees
│   ├── api-validation/       # API assumption testing
│   │   ├── .git -> proto/feature-api-validation
│   │   ├── context -> ../context/
│   │   └── [lightweight investigation apps]
│   ├── tech-feasibility/     # Performance assumption testing  
│   │   ├── .git -> proto/feature-tech-feasibility
│   │   ├── context -> ../context/
│   │   └── [performance testing apps]
│   └── arch-integration/     # Integration assumption testing
│       ├── .git -> proto/feature-arch-integration
│       ├── context -> ../context/
│       └── [integration testing apps]
└── final/                    # Production implementation (after testing)
    ├── backend-service/      # Production backend
    │   ├── .git -> feature/feature-backend
    │   ├── context -> ../context/
    │   ├── LESSONS_LEARNED.md
    │   └── [production implementation]
    ├── frontend-app/         # Production frontend
    │   ├── .git -> feature/feature-frontend  
    │   ├── context -> ../context/
    │   ├── LESSONS_LEARNED.md
    │   └── [production implementation]
    └── integration-layer/    # Production integration
        ├── .git -> feature/feature-integration
        ├── context -> ../context/
        ├── LESSONS_LEARNED.md
        └── [production implementation]
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

This assumption-driven subagent system leverages AI agent speed to validate critical assumptions before implementation, ensuring that large features are built on proven foundations rather than untested assumptions. The methodology scales to complex features by using evidence-based decisions and alternative approach testing to achieve higher quality through rapid prototyping and validation.