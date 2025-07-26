# Subtasks Planning Methodology
## Large Feature Development with Context-Aware Subagent Coordination

## EXECUTION INSTRUCTIONS FOR CLAUDE CODE

**When this command is invoked, Claude Code must:**

1. **CREATE PROJECT STRUCTURE**: Generate all required directories and files as specified
2. **ANALYZE USER REQUEST**: Break down large feature request using comprehensive analysis
3. **EXECUTE DISCOVERY COMMANDS**: Run codebase investigation and document actual findings
4. **GENERATE CONTEXT PACKAGE**: Create all context files with real analysis and research
5. **CREATE TASK FILES**: Generate individual subtask files with complete specifications
6. **SETUP WORKTREES**: Create git worktrees and directory structure
7. **WRITE ACTUAL CONTENT**: Fill all templates with real codebase analysis and context

**File Generation Requirements:**
- Create `/requests/<feature-name>/context/` directory with all context files
- Create `/requests/<feature-name>/tasks/` directory with individual task files
- Create worktree structure with proper git setup
- Fill all templates with actual investigation results and analysis

**CRITICAL**: This is not just a methodology document - it's an execution command that must produce actual files for large feature development.

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

**CLAUDE CODE ACTION**: Create `requests/<feature-name>/analysis/request-analysis.md` with actual analysis:

```markdown
## Master Request Analysis
**Original Request**: [Copy exact user request from input]
**Business Context**: [Analyze why this feature is needed from user context]
**Success Definition**: [Define how user/business will know it's complete]
**Project Phase**: [Determine actual phase from user context]
**Timeline Constraints**: [Extract any deadline pressures from request]
**Integration Scope**: [Identify actual services/systems affected by analyzing codebase]
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

**CLAUDE CODE ACTION**: Execute the discovery commands above, then create `requests/<feature-name>/context/codebase-analysis.md` with actual findings:

```markdown
## Codebase Context Documentation

### Existing Architecture Patterns
- **Service Architecture**: [Analyze actual architecture from discovered files] - Files: [actual file paths found]
- **Data Layer**: [Identify actual database/ORM from package.json and source] - Files: [actual schema/model paths]
- **API Patterns**: [Determine actual API style from route files found] - Files: [actual API implementation paths]
- **Authentication**: [Find actual auth implementation in codebase] - Files: [actual auth service paths]
- **Configuration**: [Locate actual config management system] - Files: [actual config file paths]

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

**CLAUDE CODE ACTION**: Research relevant documentation and create `requests/<feature-name>/context/external-sources.md`:

```markdown
## External Context Sources

### Primary Documentation
- **Framework Docs**: [Identify actual framework from codebase analysis] - [Research specific relevant sections] - [Document key insights for implementation]
- **Library Docs**: [Identify relevant libraries from dependencies] - [Check version compatibility] - [Document usage patterns]
- **API Standards**: [Determine applicable standards for the feature] - [Research which standards apply] - [Document implementation guidance]

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
- **Time Scope**: 15-30 minutes execution time (MAX 30 minutes)
- **Token Scope**: Under 150k tokens per task (MANDATORY)
- **Risk Ordering**: HIGH → MEDIUM → LOW risk tasks
- **Dependencies**: Minimal cross-task dependencies
- **Testability**: Each task produces testable output
- **Isolation**: Tasks can run in parallel worktrees
- **⚠️ CRITICAL**: File contention analysis mandatory (see below)

## Task Sizing Requirements (FLEXIBLE)

### Task Size Philosophy
```markdown
## Right-Sized Tasks Philosophy
- **As small as practical**: Break down when parallel execution provides benefit
- **No artificial limits**: Some tasks naturally require more time/tokens
- **Parallelization benefit**: Only split if multiple subagents can work simultaneously
- **Atomic completion**: Each task should produce complete, testable functionality
- **Boundary respect**: Tasks should respect natural code boundaries and dependencies

## Size Guidelines (Not Hard Limits)
- **Target**: 15-30 minutes per task
- **Acceptable**: Up to 60 minutes for complex, atomic units
- **Token target**: Under 150k tokens  
- **Token acceptable**: Up to 300k tokens for unavoidable complexity
- **File target**: 3-7 files per task
- **File acceptable**: Up to 15 files if they form logical unit
```

### Token Estimation Guidelines
```bash
# Estimate tokens per task component
estimate_task_tokens() {
    local task_files="$1"
    local context_size="$2"
    local implementation_complexity="$3"
    
    # Base token costs
    context_tokens=$((context_size * 1000))        # Context package size
    file_reading_tokens=$((task_files * 2000))     # ~2k tokens per file read
    implementation_tokens=$((implementation_complexity * 5000))  # 5k per complexity unit
    validation_tokens=10000                        # Testing and validation
    
    total_tokens=$((context_tokens + file_reading_tokens + implementation_tokens + validation_tokens))
    
    echo "Task Token Estimate: $total_tokens"
    
    if [[ $total_tokens -gt 150000 ]]; then
        echo "❌ TASK TOO LARGE: $total_tokens tokens > 150k limit"
        echo "SPLIT TASK: Reduce files or complexity"
        return 1
    fi
    
    echo "✅ Task within limits: $total_tokens tokens"
    return 0
}
```

### Time Estimation Matrix
| Task Type | Complexity | Files | Estimated Time | Token Range |
|-----------|------------|-------|----------------|-------------|
| Research | Low | 1-3 | 10-15 min | 20k-50k |
| Research | Medium | 3-5 | 15-25 min | 50k-100k |
| Architecture | Low | 2-4 | 15-20 min | 30k-70k |
| Architecture | Medium | 4-6 | 20-30 min | 70k-120k |
| Development | Low | 2-4 | 15-25 min | 40k-90k |
| Development | Medium | 4-6 | 25-30 min | 90k-150k |
| Testing | Low | 2-3 | 10-20 min | 25k-60k |
| Testing | Medium | 3-5 | 20-30 min | 60k-120k |
| Integration | Low | 3-5 | 15-25 min | 50k-100k |
| Integration | Medium | 5-7 | 25-30 min | 100k-150k |

### Task Sizing Validation
```bash
# MANDATORY: Validate task size before creation
validate_task_size() {
    local task_name="$1"
    local file_count="$2"
    local complexity="$3"
    
    echo "=== Task Size Validation: $task_name ==="
    
    # File count validation
    if [[ $file_count -gt 7 ]]; then
        echo "❌ Too many files: $file_count > 7 limit"
        echo "SPLIT TASK: Group related files into separate tasks"
        return 1
    fi
    
    # Time estimation
    base_time=$((file_count * 3))  # 3 minutes per file
    complexity_time=$((complexity * 8))  # 8 minutes per complexity unit
    total_time=$((base_time + complexity_time))
    
    if [[ $total_time -gt 30 ]]; then
        echo "❌ Estimated time too high: $total_time > 30 minutes"
        echo "REDUCE COMPLEXITY: Simplify scope or split task"
        return 1
    fi
    
    # Token estimation
    estimate_task_tokens "$file_count" 50 "$complexity" || return 1
    
    echo "✅ Task size validated: $file_count files, ~$total_time minutes"
    return 0
}
```

### Task Complexity Categorization
```yaml
complexity_levels:
  1_simple:
    description: "Single pattern application, minimal logic"
    examples: ["Add endpoint", "Create component", "Update config"]
    time_multiplier: 1
    
  2_moderate:
    description: "Multiple patterns, some integration"
    examples: ["Service integration", "Complex component", "Database schema"]
    time_multiplier: 2
    
  3_complex:
    description: "Multiple services, complex logic, high integration"
    examples: ["Cross-service feature", "Complex state management", "Security implementation"]
    time_multiplier: 3
    
  4_atomic_large:
    description: "Large but atomic unit that cannot be meaningfully split"
    examples: ["Database migration", "Complete API integration", "Complex algorithm implementation"]
    action: "ACCEPT LARGER SIZE - DOCUMENT BOUNDARIES"
```

## Task Scope Boundaries & "Do Not Touch" Areas

### Scope Boundary Analysis (MANDATORY)
```markdown
## Impact Boundary Mapping
For each task, define clear boundaries to prevent unnecessary investigation:

### Direct Impact Zone (MODIFY)
- **Primary Files**: Files directly modified by this task
- **Direct Dependencies**: Files that import/use the modified files
- **Test Files**: Tests that directly test the modified functionality
- **Type Definitions**: Types directly related to the changes

### Secondary Impact Zone (REVIEW ONLY)
- **Indirect Dependencies**: Files that use direct dependencies
- **Integration Points**: Services that call modified APIs
- **Related Features**: Features that might be affected by changes
- **Documentation**: Docs that need updates

### No-Touch Zone (IGNORE)
- **Unrelated Services**: Services not using modified functionality
- **Legacy Code**: Old code not connected to changes
- **Future Features**: Planned but not implemented features
- **External Dependencies**: Third-party libraries and services
```

### Boundary Documentation Template
```markdown
## Task Scope Boundaries: [Task Name]

### MODIFY Zone (Direct Changes)
```yaml
primary_files:
  - /src/components/UserProfile.tsx      # Main component being modified
  - /src/types/User.ts                   # Type definitions for user data
  - /src/services/UserService.ts         # Service methods for user operations
  - /tests/UserProfile.test.tsx          # Tests for the component

direct_dependencies:
  - /src/components/UserDashboard.tsx    # Imports UserProfile
  - /src/pages/ProfilePage.tsx           # Uses UserProfile
  - /src/hooks/useUser.ts               # Hook used by UserProfile
```

### REVIEW Zone (Check for Impact)
```yaml
check_integration:
  - /src/components/UserList.tsx         # Might use same user types
  - /src/pages/AdminPanel.tsx           # Might display user profiles
  - /src/api/userRoutes.ts              # API routes for user operations

check_documentation:
  - /docs/user-management.md            # User management documentation
  - /docs/api/user-endpoints.md         # API documentation
```

### IGNORE Zone (Do Not Touch)
```yaml
ignore_completely:
  - /src/components/Payment/*           # Payment system unrelated to user profiles
  - /src/services/EmailService.ts      # Email service not used by user profiles
  - /legacy/old-user-system/*          # Legacy code being phased out
  - /src/admin/billing/*               # Billing system separate from user profiles
  - /external/third-party-libs/*       # External libraries

ignore_search_patterns:
  - "**/node_modules/**"               # Dependencies
  - "**/build/**"                      # Build artifacts
  - "**/dist/**"                       # Distribution files
  - "**/*.min.js"                      # Minified files
  - "**/vendor/**"                     # Third-party vendor code
```
```

### Boundary Analysis Commands
```bash
# MANDATORY: Analyze actual usage boundaries before task creation
analyze_component_usage() {
    local component_file="$1"
    local component_name=$(basename "$component_file" .tsx)
    
    echo "=== Usage Analysis for $component_name ==="
    
    # Find direct imports (MODIFY zone)
    echo "DIRECT IMPORTS (MODIFY zone):"
    grep -r "import.*$component_name" src/ --include="*.ts" --include="*.tsx" | head -10
    
    # Find type usage (REVIEW zone)
    echo "TYPE USAGE (REVIEW zone):"
    grep -r ": ${component_name}Props" src/ --include="*.ts" --include="*.tsx" | head -5
    
    # Find API calls (REVIEW zone)
    echo "API CALLS (REVIEW zone):"
    grep -r "api.*${component_name,,}" src/ --include="*.ts" --include="*.tsx" | head -5
    
    # Count total usage
    local usage_count=$(grep -r "$component_name" src/ --include="*.ts" --include="*.tsx" | wc -l)
    echo "TOTAL USAGE: $usage_count occurrences"
    
    if [[ $usage_count -lt 10 ]]; then
        echo "✅ LIMITED SCOPE: Component has limited usage - safe for focused changes"
    elif [[ $usage_count -lt 50 ]]; then
        echo "⚠️ MODERATE SCOPE: Review listed files for impact"
    else
        echo "❌ BROAD SCOPE: Consider splitting task or extensive impact analysis"
    fi
}

# Usage: analyze_component_usage "src/components/UserProfile.tsx"
```

### Scope Boundary Enforcement
```bash
# Prevent scope creep during task execution
validate_task_scope() {
    local task_dir="$1"
    local boundary_file="$task_dir/scope-boundaries.yaml"
    
    echo "=== Scope Boundary Validation ==="
    
    if [[ ! -f "$boundary_file" ]]; then
        echo "❌ Missing scope boundaries file"
        echo "Create $boundary_file with MODIFY/REVIEW/IGNORE zones"
        return 1
    fi
    
    # Check if task is modifying files outside MODIFY zone
    local modified_files=$(git -C "$task_dir" diff --name-only)
    local boundary_violations=0
    
    while IFS= read -r file; do
        # Check if file is in IGNORE zone
        if grep -q "ignore_completely" "$boundary_file" | grep -q "$file"; then
            echo "❌ BOUNDARY VIOLATION: $file is in IGNORE zone"
            boundary_violations=$((boundary_violations + 1))
        fi
    done <<< "$modified_files"
    
    if [[ $boundary_violations -eq 0 ]]; then
        echo "✅ No boundary violations detected"
        return 0
    else
        echo "❌ $boundary_violations boundary violations found"
        return 1
    fi
}
```

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

**CLAUDE CODE ACTION**: For each identified subtask, create `requests/<feature-name>/tasks/XX-task-name.md` with actual content:

```markdown
# Task: <Actual-Task-Name>
**Generated from Master Planning**: [Current Date]
**Context Package**: `/requests/<feature-name>/context/`
**Next Phase**: [subtasks-execute.md](subtasks-execute.md)

## Task Sizing Assessment (MANDATORY)
**File Count**: [X files] - [Within target/Justified by atomicity]
**Estimated Time**: [X minutes] - [Target: 15-30min, Acceptable: up to 60min]
**Token Estimate**: [X tokens] - [Target: <150k, Acceptable: <300k]
**Complexity Level**: [1-4] - [Simple/Moderate/Complex/Atomic-Large]
**Parallelization Benefit**: [HIGH/MEDIUM/LOW] - [Justification for task split]
**Atomicity Assessment**: ✅ ATOMIC - Cannot be meaningfully split further
**Boundary Analysis**: ✅ CLEAR - MODIFY/REVIEW/IGNORE zones defined

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
# Total: Flexible based on task atomicity
```

**Existing Patterns to Follow**:
- [File path] - [Pattern name] - [How to apply to this task]
- [File path] - [Pattern name] - [How to apply to this task]

**Dependencies Context**:
- [Package@version] - [Documentation URL] - [Specific usage for this task]

### Task Scope Boundaries (MANDATORY)
**MODIFY Zone** (Direct Changes):
```yaml
primary_files:
  - /src/components/TargetComponent.tsx    # Main file being modified
  - /src/types/ComponentTypes.ts           # Related type definitions
  - /tests/TargetComponent.test.tsx        # Direct tests

direct_dependencies:
  - /src/components/ParentComponent.tsx    # Imports TargetComponent
  - /src/hooks/useTargetData.ts           # Hook used by component
  - /src/services/TargetService.ts        # Service methods
```

**REVIEW Zone** (Check for Impact):
```yaml
check_integration:
  - /src/components/RelatedComponent.tsx   # Might use same patterns
  - /src/pages/ComponentPage.tsx          # Page that uses component
  - /src/api/componentRoutes.ts           # API routes

check_documentation:
  - /docs/components/target-component.md   # Component documentation
```

**IGNORE Zone** (Do Not Touch):
```yaml
ignore_completely:
  - /src/components/Payment/*             # Unrelated payment system
  - /src/services/EmailService.ts        # Email service not used
  - /legacy/old-components/*             # Legacy code
  - /src/admin/billing/*                 # Billing system
  - /external/third-party-libs/*        # External libraries

ignore_search_patterns:
  - "**/node_modules/**"                 # Dependencies
  - "**/build/**"                        # Build artifacts
  - "**/dist/**"                         # Distribution files
  - "**/*.min.js"                        # Minified files
```

**Boundary Analysis Results**:
- **Usage Count**: [X] occurrences found
- **Scope Assessment**: [LIMITED/MODERATE/BROAD] scope
- **Impact Radius**: [X] files in MODIFY zone, [Y] files in REVIEW zone

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

**CLAUDE CODE ACTION**: Execute these git commands to set up the project structure:

```bash
# Master feature branch setup
git checkout -b feature/<actual-feature-name>
git push -u origin feature/<actual-feature-name>

# Create context package directory
mkdir -p requests/<actual-feature-name>/context
mkdir -p requests/<actual-feature-name>/tasks
mkdir -p requests/<actual-feature-name>/analysis

# Individual task worktrees with context access
git worktree add ~/work/worktrees/<actual-feature-name>/<task-id> feature/<actual-feature-name>

# Context sharing setup
ln -s $(pwd)/requests/<actual-feature-name>/context ~/work/worktrees/<actual-feature-name>/<task-id>/context
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

## Task Sizing Assessment (MANDATORY)
- [ ] **Right-Sized**: All tasks sized appropriately for atomic completion
- [ ] **Parallelization Justified**: Task splits provide genuine parallel execution benefit
- [ ] **Atomicity Verified**: Tasks cannot be meaningfully split further
- [ ] **Complexity Assessed**: All tasks categorized by complexity level (1-4)
- [ ] **Boundary Analysis**: All tasks have clear MODIFY/REVIEW/IGNORE zones
- [ ] **Scope Limitations**: All tasks have explicit "do not touch" areas defined

## Scope Boundary Validation (MANDATORY)
- [ ] **Usage Analysis**: Component/service usage analyzed for each task
- [ ] **Impact Radius**: Clear boundaries defined for each task
- [ ] **No-Touch Zones**: Explicit ignore patterns documented
- [ ] **Scope Enforcement**: Boundary validation commands provided
- [ ] **Conflict Prevention**: File contention analysis completed with boundaries

## Context Quality Validation
- [ ] **Codebase Patterns**: Similar implementations identified and documented
- [ ] **External Standards**: Applicable standards documented with URLs
- [ ] **Architecture Integration**: Clear integration points defined
- [ ] **Implementation Guidance**: Concrete patterns for subagents to follow

## Handoff to Execution Phase
**Master Planning Complete** → **Next: [subtasks-execute.md](subtasks-execute.md)**

### FILES CREATED - VERIFICATION REQUIRED ✅
**User must verify these files exist before proceeding:**

1. **Context Package Files**:
   - **Location**: `/requests/<feature-name>/context/`
   - **Files**: `codebase-analysis.md`, `external-sources.md`, `architecture-decisions.md`, `implementation-patterns.md`
   - **Verify with**: `ls -la requests/*/context/ && find requests/*/context/ -name "*.md" -type f`

2. **Individual Task Files**:
   - **Location**: `/requests/<feature-name>/tasks/`
   - **Files**: `01-task-name.md`, `02-task-name.md`, etc. (one per subtask)
   - **Verify with**: `ls -la requests/*/tasks/ && find requests/*/tasks/ -name "*.md" -type f`

3. **Worktree Structure**:
   - **Location**: `~/work/worktrees/<feature-name>/`
   - **Contains**: Individual worktree directories for each task
   - **Verify with**: `ls -la ~/work/worktrees/*/`

4. **Master Planning Summary**:
   - **Location**: `requests/<feature-name>/planning-summary.md`
   - **Contains**: Overall feature breakdown and task coordination plan
   - **Verify with**: `cat requests/*/planning-summary.md | head -20`

**⚠️ CRITICAL**: The next agent (execution phase) will start with ZERO CONTEXT from this conversation. It will ONLY have access to:
- The files created above
- The current codebase state
- No memory of this planning discussion
- No access to external research conducted during planning

**⚠️ STOP AND VERIFY**: User must confirm all files exist at specified locations before proceeding to execution phase.

**Next Step**: Only proceed to [subtasks-execute.md](subtasks-execute.md) after user confirms file creation.

## CLAUDE CODE EXECUTION SUMMARY

**When `/subtasks-plan` command is invoked, Claude Code must execute these steps in order:**

1. **Extract feature name** from user request
2. **Create directory structure**:
   ```bash
   mkdir -p requests/<feature-name>/{context,tasks,analysis}
   ```
3. **Execute codebase discovery commands** and analyze results thoroughly
4. **Create all context files** with actual content (not templates):
   - `requests/<feature-name>/analysis/request-analysis.md`
   - `requests/<feature-name>/context/codebase-analysis.md`
   - `requests/<feature-name>/context/external-sources.md` 
   - `requests/<feature-name>/context/architecture-decisions.md`
   - `requests/<feature-name>/context/implementation-patterns.md`
5. **Break down large feature into subtasks** using the comprehensive methodology
6. **Generate subtask files** `requests/<feature-name>/tasks/XX-task-name.md` for each subtask
7. **Set up git structure** with feature branch and worktrees
8. **Create planning summary** with overall coordination plan
9. **Verify all files exist** before completing

**CRITICAL REQUIREMENTS**:
- This command is for LARGE features (>10 files, >16 hours, multiple domains)
- All discovery commands must be executed and results documented
- Context package must contain actual analysis from codebase investigation
- Subtask files must be complete with boundaries, context, and specifications
- Worktree structure must be properly configured for parallel development

**OUTPUT VERIFICATION**:
- All context files exist with real content
- All subtask files exist with complete specifications  
- Directory structure properly created
- Git worktrees configured
- Planning summary documents overall approach
```

This planning methodology ensures that every subagent receives comprehensive, contextual information needed for effective parallel development while maintaining consistency across the entire feature implementation.