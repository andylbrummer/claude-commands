# Subtasks Planning Methodology
## Large Feature Development with Context-Aware Subagent Coordination

## Prerequisites Check

### MANDATORY: Before Planning Subtasks
```markdown
## Previous Work Verification ✅
- [ ] **Previous Tasks Complete**: All prior individual tasks have been completed and archived
- [ ] **Previous Subtasks Complete**: All prior large subtask features have been completed and archived
- [ ] **Files Archived**: All previous task/subtask files have been moved to archive folders
- [ ] **Clean Slate**: No incomplete tasks or subtasks remain in active folders
- [ ] **Dependencies Resolved**: All blocking dependencies from previous work resolved

## Large Feature Prerequisites
- [ ] **Scope Justified**: Feature complexity justifies subtask methodology (>10 files, >16 hours, multiple domains)
- [ ] **Architecture Readiness**: System architecture can support large feature development
- [ ] **Resource Availability**: Multiple subagents and worktrees can be supported
- [ ] **Integration Capability**: Cross-service integration requirements are understood
- [ ] **Context Sources Available**: Sufficient codebase and external context sources exist

## Archive and Knowledge Verification
- [ ] **Archive Structure**: Previous work properly archived in `/archive/features/YYYY-MM-DD-feature-name/`
- [ ] **Completion Documentation**: Previous features include completion reports and retrospectives
- [ ] **Knowledge Transfer**: Key learnings from previous large features documented and accessible
- [ ] **Context Updates**: Any architectural or process improvements from previous work incorporated
- [ ] **Pattern Library**: Successful patterns from previous features documented for reuse

**⚠️ STOP**: Do not proceed with subtask planning until ALL previous tasks/subtasks are complete and archived
```

## Quick Start Decision Tree

### Choose Your Approach:
- **📝 Small Feature** (1-3 files, < 4 hours) → Use [tasks-plan.md](tasks-plan.md) instead
- **🔧 Medium Feature** (4-10 files, 4-16 hours) → Use [tasks-plan.md](tasks-plan.md) with breakdown
- **🏗️ Large Feature** (>10 files, >16 hours, multiple domains) → Full subtasks methodology below
- **🔄 Multi-Service Integration** → Mandatory: Full methodology + cross-service coordination

---

## Phase 1: Master Request Analysis & Context Capture

### 1. Deep Request Understanding
```markdown
## Master Request Analysis
**Original Request**: [Copy exact user request]
**Business Context**: [Why this feature is needed]
**Success Definition**: [How user/business will know it's complete]
**Project Phase**: [Prototype/MVP/Production]
**Timeline Constraints**: [Any deadline pressures]
**Integration Scope**: [Services/systems affected]
```

### 2. Comprehensive Codebase Investigation

#### Required Context Discovery (Mandatory):
```bash
# Architecture Discovery
find . -name "*.ts" -o -name "*.js" -o -name "*.go" -o -name "*.py" | head -50
find . -name "package.json" -o -name "go.mod" -o -name "requirements.txt"
find . -name "*.config.*" -o -name "docker*" -o -name "*compose*"

# Feature Area Mapping
grep -r "similar_functionality" --include="*.ts" --include="*.js" src/
grep -r "related_patterns" --include="*.go" --include="*.py" .
git log --oneline --grep="feature_area" -20

# Integration Point Discovery
grep -r "api.*endpoint" src/ docs/
grep -r "service.*interface" src/ docs/
find . -name "*schema*" -o -name "*interface*" -o -name "*contract*"

# Documentation Context
find . -name "README*" -o -name "*.md" docs/ | head -20
grep -r "architectural.*decision" docs/
grep -r "integration.*guide" docs/
```

#### Context Capture Requirements:
```markdown
## Codebase Context Documentation

### Existing Architecture Patterns
- **Service Architecture**: [Microservices/Monolith/Hybrid] - Files: [paths]
- **Data Layer**: [Database type, ORM, migration patterns] - Files: [paths]
- **API Patterns**: [REST/GraphQL/gRPC] - Files: [example implementations]
- **Authentication**: [Current auth system] - Files: [auth service paths]
- **Configuration**: [How config is managed] - Files: [config file paths]

### Similar Feature Implementations
- **Feature A**: [Path] - [Pattern used] - [Relevance to new feature]
- **Feature B**: [Path] - [Pattern used] - [Relevance to new feature]
- **Integration Pattern**: [Path] - [How services connect] - [Applicability]

### Dependency Analysis
- **Core Dependencies**: [Package@version] - [Documentation URL] - [Usage pattern]
- **Dev Dependencies**: [Testing frameworks, build tools] - [Version constraints]
- **External Services**: [APIs, databases] - [Integration patterns] - [Documentation URLs]

### File Dependency Mapping
```yaml
high_change_areas:
  - /src/services/auth/: [auth-related changes]
  - /src/api/routes/: [new endpoints]
  - /src/database/migrations/: [schema changes]
  
medium_change_areas:
  - /src/types/: [type definitions]
  - /src/utils/: [shared utilities]
  - /tests/: [test suites]

low_change_areas:
  - /src/config/: [configuration updates]
  - /docs/: [documentation updates]
```
```

### 3. External Context Research & Documentation

#### Required External Sources (Document All):
```markdown
## External Context Sources

### Primary Documentation
- **Framework Docs**: [Next.js](https://nextjs.org/docs) - [Specific sections relevant] - [Key insights]
- **Library Docs**: [Library](URL) - [Version compatibility] - [Usage patterns]
- **API Standards**: [OpenAPI](URL) - [Which standards apply] - [Implementation guidance]

### Industry Standards & Best Practices
- **Security Standards**: [OWASP](URL) - [Applicable guidelines] - [Threat model implications]
- **Performance Standards**: [Web Vitals](URL) - [Target metrics] - [Optimization techniques]
- **Accessibility**: [WCAG](URL) - [Compliance level] - [Implementation requirements]

### Reference Implementations
- **Similar Features**: [GitHub Repo](URL) - [Pattern demonstrated] - [Adaptation needed]
- **Integration Examples**: [Example](URL) - [Integration pattern] - [Lessons learned]
- **Architecture Examples**: [System Design](URL) - [Scalability patterns] - [Trade-offs]

### Standards Applied
- **Coding Standards**: [Project Style Guide](URL) - [Specific rules for this feature]
- **API Design**: [REST Guidelines](URL) - [Naming conventions] - [Error handling]
- **Testing Standards**: [Testing Strategy](URL) - [Coverage requirements] - [Test patterns]
```

## Phase 2: Master Feature Breakdown & Task Decomposition

### Architecture-First Decomposition
```markdown
## Feature Architecture Design

### System Integration Points
- **Data Flow**: [How data moves through system] - [Services involved]
- **API Contracts**: [New endpoints] - [Request/response schemas] - [Versioning strategy]
- **Service Boundaries**: [Which services change] - [Communication patterns]
- **Database Impact**: [Schema changes] - [Migration strategy] - [Performance impact]

### Risk Assessment by Component
- **Authentication Changes**: HIGH RISK - [Security implications] - [Mitigation: feature flags]
- **Database Schema**: MEDIUM RISK - [Migration complexity] - [Mitigation: backwards compatibility]
- **API Changes**: MEDIUM RISK - [Breaking changes] - [Mitigation: versioning]
- **UI Components**: LOW RISK - [User impact] - [Mitigation: progressive rollout]
```

### Task Decomposition Strategy
```markdown
## Task Breakdown Rules
- **File Scope**: 3-7 files per task maximum
- **Time Scope**: 15-45 minutes execution time
- **Risk Ordering**: HIGH → MEDIUM → LOW risk tasks
- **Dependencies**: Minimal cross-task dependencies
- **Testability**: Each task produces testable output
- **Isolation**: Tasks can run in parallel worktrees
- **⚠️ CRITICAL**: File contention analysis mandatory (see below)

## File Contention Analysis (MANDATORY)
**Before task decomposition, analyze file edit conflicts:**

### Step 1: File Modification Prediction
```bash
# Predict file modifications per task area
echo "Analyzing potential file edits per domain..."

# API changes typically modify:
# - /src/api/routes/*.ts
# - /src/types/api.ts  
# - /src/services/*.service.ts
# - /tests/api/*.test.ts

# Database changes typically modify:
# - /src/database/migrations/*.sql
# - /src/types/database.ts
# - /src/models/*.model.ts
# - /tests/models/*.test.ts

# UI changes typically modify:
# - /src/components/*.tsx
# - /src/types/ui.ts
# - /src/styles/*.css
# - /tests/components/*.test.tsx
```

### Step 2: Contention Risk Matrix
| File Path | Task A | Task B | Task C | Conflict Risk | Resolution Strategy |
|-----------|--------|--------|--------|---------------|-------------------|
| `/src/types/api.ts` | Edit | Edit | - | HIGH | Sequential: A→B, interface versioning |
| `/src/config/app.ts` | Edit | - | Edit | MEDIUM | Namespace separation |
| `/src/utils/helpers.ts` | Edit | Edit | Edit | CRITICAL | Extract to separate tasks first |

### Step 3: Conflict Prevention Strategies
**CRITICAL File Types Requiring Special Handling:**
- **Type Definition Files** (`*.types.ts`): Create interface versioning strategy
- **Configuration Files** (`config/*.ts`): Use namespace separation
- **Shared Utilities** (`utils/*.ts`): Extract modifications to separate pre-tasks  
- **Database Schemas** (`*.schema.ts`): Coordinate migration ordering
- **API Contracts** (`api/*.ts`): Implement contract versioning

### Step 4: Task Ordering for Conflict Prevention
```yaml
# Dependency ordering based on file contention analysis
foundation_tasks:
  - 01-extract-shared-utils    # Move shared utility changes to separate files
  - 02-create-interface-base   # Create base interfaces other tasks can extend
  
parallel_safe_tasks:
  - 03-dev-backend-auth       # Minimal shared file overlap
  - 03-dev-frontend-ui        # Different file domains
  - 03-dev-database-schema    # Isolated schema changes
  
integration_tasks:
  - 04-integrate-interfaces   # Merge interface changes
  - 05-resolve-conflicts      # Handle any remaining conflicts
```

## Task Categories & Personas
- **Research Tasks** (`01-research-*`) → Research Analyst
- **Architecture Tasks** (`02-arch-*`) → System Architect  
- **Development Tasks** (`03-dev-*`) → Software Engineer
- **Testing Tasks** (`04-test-*`) → QA Engineer
- **Review Tasks** (`05-review-*`) → Senior Engineer
- **Integration Tasks** (`06-integration-*`) → DevOps Engineer
- **Documentation Tasks** (`07-docs-*`) → Technical Writer
```

## Phase 3: Context Package Creation

### Master Context Package Structure
```markdown
## Context Package: /requests/<feature-name>/context/

### codebase-analysis.md
- Complete file dependency mapping
- Architecture pattern documentation
- Similar implementation references
- Integration point analysis

### external-sources.md  
- All documentation URLs with relevance notes
- Standards and guidelines with applicable sections
- Reference implementations with adaptation notes
- Version compatibility matrices

### architecture-decisions.md
- Design decisions with rationale
- Trade-offs and alternatives considered
- Integration patterns chosen
- Risk mitigation strategies

### implementation-patterns.md
- Code patterns to follow from existing codebase
- Naming conventions and style guidelines
- Error handling patterns
- Testing patterns and requirements
```

## Phase 4: Task File Generation

### Context-Aware Task Template
```markdown
# Task: <Task-Name>
**Generated from Master Planning**: [Date]
**Context Package**: `/requests/<feature>/context/`
**Next Phase**: [subtasks-execute.md](subtasks-execute.md)

## Persona Assignment
**Persona**: [Software Engineer/System Architect/etc.]
**Expertise Required**: [Specific skills needed]
**Worktree**: `~/work/worktrees/<feature>/<task-id>/`

## Context Summary
**Risk Level**: [HIGH/MEDIUM/LOW]
**Integration Points**: [Which other tasks this connects to]
**Architecture Pattern**: [From context package - which pattern to follow]
**Similar Reference**: [File path of similar implementation]

### Codebase Context (from master analysis)
**Files in Scope**:
```yaml
read_files:   [/specific/file1.ts, /specific/file2.ts]
modify_files: [/specific/file3.ts] 
create_files: [/new/file.ts, /test/file.test.ts]
# Total: <7 files maximum
```

**Existing Patterns to Follow**:
- [File path] - [Pattern name] - [How to apply to this task]
- [File path] - [Pattern name] - [How to apply to this task]

**Dependencies Context**:
- [Package@version] - [Documentation URL] - [Specific usage for this task]

### External Context Sources (from master research)
**Primary Documentation**:
- [Title](URL) - [Specific section relevant to this task] - [How to apply]
- [Title](URL) - [Specific section relevant to this task] - [How to apply]

**Standards Applied**:
- [Standard](URL) - [Which parts apply to this task] - [Implementation checklist]

**Reference Implementation**:
- [Example](URL) - [What pattern to adapt] - [Specific changes needed]

## Task Requirements
**Objective**: [Clear, specific goal]
**Success Criteria**:
- [ ] [Specific measurable outcome referencing context]
- [ ] [Implementation follows pattern from similar file X]
- [ ] [External standard Y properly applied]
- [ ] [Integration with task Z verified]

**Validation Commands**:
```bash
# Context Application Verification
grep -q "expected_pattern" /modified/file.ts    # Pattern properly applied
npm test path/to/specific.test.ts               # Tests pass
npm run lint /modified/file.ts                  # Style follows project standards
npm run integration-test -- --grep "feature"   # Integration verified
```

## Risk Mitigation (from master analysis)
**High-Risk Mitigations**:
- [Specific risk] - [Mitigation strategy] - [Fallback plan]
- [Context-informed risk] - [Pattern-based solution] - [Rollback procedure]

**Context Validation**:
- [ ] Similar pattern from [file] successfully adapted
- [ ] External standard [name] properly implemented  
- [ ] Integration pattern matches [reference implementation]

## Integration with Other Tasks
**Dependencies**: [Which tasks must complete first]
**Integration Points**: [How this connects to parallel tasks]
**Shared Context**: [Context elements shared with other tasks]

## Documentation from Master Context
[Relevant sections from context package]

## Execution Notes
- **Start Pattern**: Use [specific file] as implementation template
- **Key Context**: Pay special attention to [specific context element]
- **Integration Test**: Verify [specific integration point] works
- **Review Focus**: [Specific areas for extra review attention]
```

## Phase 5: Worktree & Git Setup

### Git Workflow for Context-Aware Development
```bash
# Master feature branch setup
git checkout -b feature/<feature-name>
git push -u origin feature/<feature-name>

# Create context package directory
mkdir -p requests/<feature-name>/context
mkdir -p requests/<feature-name>/tasks

# Individual task worktrees with context access
git worktree add ~/work/worktrees/<feature>/<task-id> feature/<feature-name>

# Context sharing setup
ln -s $(pwd)/requests/<feature-name>/context ~/work/worktrees/<feature>/<task-id>/context
```

### Worktree Structure
```
~/work/worktrees/<feature-name>/
├── 01-research-api/          # Research subagent
│   ├── .git -> feature branch
│   ├── context -> shared context package
│   └── [source files]
├── 02-arch-design/           # Architecture subagent  
│   ├── .git -> feature branch
│   ├── context -> shared context package
│   └── [source files]
├── 03-dev-backend/           # Backend development
│   ├── .git -> feature branch
│   ├── context -> shared context package
│   └── [source files]
└── shared-context/           # Master context package
    ├── codebase-analysis.md
    ├── external-sources.md
    ├── architecture-decisions.md
    └── implementation-patterns.md
```

## Master Planning Output

### Planning Complete Checklist
```markdown
## Ready for Subagent Execution
- [ ] **Context Package Complete**: All required context documented
- [ ] **Task Files Generated**: Each task has complete context reference
- [ ] **Worktree Structure**: Git worktrees ready for parallel development
- [ ] **Integration Plan**: Cross-task dependencies mapped
- [ ] **Risk Mitigation**: High-risk items have specific strategies
- [ ] **Source Validation**: All external sources verified and accessible

## Context Quality Validation
- [ ] **Codebase Patterns**: Similar implementations identified and documented
- [ ] **External Standards**: Applicable standards documented with URLs
- [ ] **Architecture Integration**: Clear integration points defined
- [ ] **Implementation Guidance**: Concrete patterns for subagents to follow

## Handoff to Execution Phase
**Master Planning Complete** → **Next: [subtasks-execute.md](subtasks-execute.md)**

**Context Package Location**: `/requests/<feature-name>/context/`
**Task Files Location**: `/requests/<feature-name>/tasks/`
**Worktree Base**: `~/work/worktrees/<feature-name>/`
```

This planning methodology ensures that every subagent receives comprehensive, contextual information needed for effective parallel development while maintaining consistency across the entire feature implementation.