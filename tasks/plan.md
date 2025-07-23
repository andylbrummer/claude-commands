# Task Planning Methodology

## Prerequisites Check

### MANDATORY: Before Planning Tasks
```markdown
## Previous Work Verification ✅
- [ ] **Previous Tasks Complete**: All prior tasks have been completed and marked as COMPLETE
- [ ] **Previous Subtasks Complete**: All prior subtasks have been completed and archived
- [ ] **Files Archived**: All previous task/subtask files have been moved to archive folders
- [ ] **Clean Slate**: No incomplete tasks or subtasks remain in active folders
- [ ] **Dependencies Resolved**: All blocking dependencies from previous work resolved

## Archive Verification
- [ ] **Archive Structure**: Previous work properly archived in `/archive/YYYY-MM-DD-task-name/`
- [ ] **Completion Documentation**: Previous tasks include completion reports and retrospectives
- [ ] **Knowledge Transfer**: Key learnings from previous work documented and accessible
- [ ] **Context Updates**: Any context updates from previous work incorporated

**⚠️ STOP**: Do not proceed with planning until ALL previous tasks/subtasks are complete and archived
```

## Quick Start Decision Tree

### Choose Your Approach:
- **🚀 Simple Task** (< 2 hours, low risk) → Skip to [tasks-execute.md](tasks-execute.md) 
- **⚡ Medium Task** (2-8 hours, some complexity) → Use Light Planning below + [tasks-execute.md](tasks-execute.md)
- **🎯 Complex Task** (> 8 hours, high risk/impact) → Full methodology below
- **🔄 Replacing Functionality** → Mandatory: Full methodology + feature flags

### Light Planning (5-minute version):
```markdown
## Quick Plan Template
- **Risk Level**: Low/Medium/High
- **Files Affected**: [List 3-7 specific file paths]
- **Validation Command**: `[single command to verify success]`
- **Success Criteria**: [1-3 specific checkable outcomes]
- **Project Phase**: Prototype/MVP/Production
- **Feature Flag Needed**: Yes/No (Yes if replacing anything)
- **Context Sources**: [Key files/docs that informed approach]
- **Similar Patterns**: [Existing code that does similar things]
```

### Real Example:
```markdown
## Quick Plan: Add User Avatar Display
- **Risk Level**: Low
- **Files Affected**: src/components/UserProfile.tsx, src/types/User.ts, tests/UserProfile.test.tsx
- **Validation Command**: `npm test -- UserProfile`
- **Success Criteria**: Avatar shows when user has image, fallback shows when no image, tests pass
- **Project Phase**: Prototype
- **Feature Flag Needed**: No
- **Context Sources**: src/components/UserCard.tsx (existing avatar pattern), docs/design-system.md
- **Similar Patterns**: UserCard.tsx uses same image fallback logic, consistent with design system
```

**→ Next Step**: Take this to [tasks-execute.md](tasks-execute.md) and start implementing

---

## Pre-Planning Analysis Phase (Full Methodology)

### 1. Comprehensive Request Analysis
```markdown
## Request Breakdown
- **Primary Objective**: [What is the main goal?]
- **Secondary Objectives**: [Supporting goals or side effects]
- **User Intent**: [Why does the user want this?]
- **Success Definition**: [How will we know it's complete?]
- **Scope Boundaries**: [What's explicitly in/out of scope?]

## Project Phase Assessment
- **Current Phase**: [Prototype/MVP/Beta/Production]
- **Data Strategy**: [Recreate vs Migrate test data]
- **Backwards Compatibility**: [Required/Nice-to-have/Not needed]
- **Production Readiness**: [Full/Minimal/Prototype-only]
- **User Base**: [Internal/Limited beta/Full production]

## Replacement Strategy (if replacing existing functionality)
- **Feature Flag Required**: YES (mandatory for all replacements)
- **Rollback Plan**: [How to switch back if issues arise]
- **Parallel Running**: [How long to run both versions]
- **Verification Criteria**: [What proves new version works]
- **Removal Timeline**: [When it's safe to remove old version]
```

### 2. Extensive Codebase Investigation & Context Capture

#### Required Context Sources (All must be documented):
```bash
# Codebase Context
find . -name "*.js" -o -name "*.ts" -o -name "*.py" | head -20
grep -r "keyword" --include="*.js" --include="*.ts" src/
git log --oneline -10  # Recent changes context
npm ls --depth=0       # Dependencies

# Project Documentation Context  
ls docs/ README.md CONTRIBUTING.md
grep -r "keyword" docs/
git log --oneline -- docs/ | head -5

# External Dependencies Context
npm info package-name  # For each major dependency
curl -s https://api.github.com/repos/owner/repo/releases/latest  # Latest versions
```

**Context Capture Requirements (Mandatory):**
- [ ] **Existing Functionality**: Document what already exists, with file paths
- [ ] **Similar Implementations**: Find and reference existing patterns in codebase
- [ ] **Dependencies**: Version constraints, known issues, migration paths
- [ ] **Architecture Context**: How this fits into existing system design  
- [ ] **Recent Changes**: What has changed recently in related areas
- [ ] **Documentation Context**: Existing docs that relate to this change
- [ ] **External Standards**: Industry patterns, framework conventions
- [ ] **Performance Context**: Current performance characteristics and constraints
- [ ] **Security Context**: Existing security patterns and requirements

### 3. Web Research & Documentation Review

#### Required External Research (Document all sources):
```markdown
## Research Areas with Source Documentation
- **Framework Documentation**: Latest version capabilities and breaking changes
  - Source URLs: [Specific documentation links]
  - Version compatibility: [Current vs target versions]
  - Breaking changes: [What changes between versions]
  
- **Best Practices**: Industry standards for the specific functionality  
  - Source URLs: [Industry guides, authoritative sources]
  - Standards applied: [Which standards are relevant]
  - Deviation justifications: [Why we differ from standards if applicable]
  
- **Security Considerations**: OWASP guidelines, vulnerability databases
  - Source URLs: [OWASP links, CVE databases, security advisories]  
  - Applicable guidelines: [Which security patterns apply]
  - Threat model: [What threats this change addresses/introduces]
  
- **Performance Patterns**: Benchmarks, optimization techniques
  - Source URLs: [Performance studies, benchmarks]
  - Baseline metrics: [Current performance characteristics]
  - Target metrics: [Expected performance after change]
  
- **Integration Patterns**: How similar systems solve this problem
  - Source URLs: [Example implementations, case studies]
  - Alternative approaches: [Other ways to solve this problem]
  - Trade-offs: [Why chosen approach vs alternatives]
```

#### Context Source Documentation Template:
```markdown
## External Context Sources
### Primary Documentation:
- [Title](URL) - [Why relevant] - [Key insights]
- [Title](URL) - [Why relevant] - [Key insights]

### Code Examples/References:
- [Repo/File](URL) - [What pattern it demonstrates]
- [Repo/File](URL) - [What pattern it demonstrates]

### Standards/Guidelines:
- [Standard Name](URL) - [Which parts apply]
- [Standard Name](URL) - [Which parts apply]

### Dependencies/Libraries:
- [Library Name](Documentation URL) - [Version] - [Usage pattern]
- [Library Name](Documentation URL) - [Version] - [Usage pattern]
```

### 4. User Context Interrogation
```markdown
## User Interview Questions
- **Environment**: "What's your current tech stack and constraints?"
- **Scale**: "How many users/requests/data volume are we expecting?"
- **Timeline**: "When do you need this completed?"
- **Preferences**: "Any specific libraries or patterns you prefer/avoid?"
- **Success Criteria**: "How will you test that this works correctly?"
- **Integration**: "What other systems need to work with this?"
```

## Risk Assessment & Derisking

### High-Risk Categories
```markdown
⚠️ **CRITICAL RISKS** - Require approval before proceeding:

🔒 **Security Risks**
- Authentication/authorization changes
- Input validation modifications  
- Credential handling
- Database access pattern changes
- External API integrations

💥 **Breaking Change Risks**
- Public API modifications
- Database schema changes
- Configuration format changes
- Dependency major version updates

⚡ **Performance Risks**
- Database query modifications
- Large data processing
- Memory-intensive operations
- Network-heavy operations

🔧 **Integration Risks**
- Third-party service dependencies
- Cross-service communication changes
- State management modifications
```

### Risk Mitigation Strategies
```markdown
## Risk Level: HIGH
**Strategy**: Minimal Viable Implementation + Mandatory Safety
- Create smallest possible proof-of-concept first
- Comprehensive error handling and logging
- **MANDATORY: Feature flags for any functionality replacement**
- Extensive test coverage (unit + integration)
- Rollback plan documented and tested
- Parallel operation period planned

## Risk Level: MEDIUM  
**Strategy**: Incremental Development
- Break into smaller, testable chunks
- Mock external dependencies
- Performance monitoring
- Backwards compatibility maintained
- Feature flags for user-facing changes

## Risk Level: LOW
**Strategy**: Standard Development
- Follow established patterns
- Basic test coverage
- Standard error handling
```

### Data Strategy Decision Framework
```markdown
## Prototype Phase: Recreate vs Migrate
**Choose RECREATE when**:
- Data volume is small (<1000 records)
- Schema changes are extensive
- Data relationships are simple
- Test data generation is automated
- Migration complexity > recreation time

**Choose MIGRATE when**:
- Data has complex business logic
- Historical data is valuable for testing
- Relationships are complex and hard to recreate
- Migration scripts needed for production anyway

## Production Phase: Always Migrate
- Comprehensive migration strategy required
- Zero-downtime approach planned
- Rollback procedures tested
- Data integrity validation automated
```

## Task Definition Framework

### File Scope Methodology
```markdown
## File Analysis
**Existing Files**: [List files that need modification]
**New Files**: [List files that need creation]
**Dependencies**: [Files that might be affected indirectly]
**Test Files**: [Corresponding test files needed]

## Scope Rules
- Max 3-7 files per task (for complexity management)
- Group logically related changes together
- Separate high-risk changes into isolated tasks
- Order tasks by dependency chain
```

### Task Sizing Guidelines
```markdown
## Task Size Philosophy
- **As small as practical**: Break down when it provides clear benefit
- **No artificial limits**: Some tasks naturally require more time/tokens
- **Atomic completion**: Each task should produce complete, testable functionality
- **Boundary respect**: Tasks should respect natural code boundaries

## Size Guidelines (Flexible)
- **Target Time**: 15-30 minutes per task
- **Acceptable Time**: Up to 60 minutes for complex, atomic units
- **Token Target**: Under 150k tokens per task
- **Token Acceptable**: Up to 300k tokens for unavoidable complexity
- **File Target**: 3-7 files per task
- **File Acceptable**: Up to 15 files if they form logical unit

## Task Sizing Assessment
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
        echo "⚠️ LARGE TASK: $total_tokens tokens > 150k target"
        if [[ $total_tokens -gt 300000 ]]; then
            echo "❌ TASK TOO LARGE: $total_tokens tokens > 300k limit"
            echo "MUST SPLIT: Reduce files or complexity"
            return 1
        fi
    fi
    
    echo "✅ Task size acceptable: $total_tokens tokens"
    return 0
}
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

### Usage Analysis Commands
```bash
# MANDATORY: Analyze actual usage boundaries before task execution
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
```

### Completion Criteria Template
```markdown
## Success Criteria for [Task Name]
**Functional Requirements:**
- [ ] Feature works as specified: [specific behavior]
- [ ] Error cases handled: [list expected error scenarios]
- [ ] Integration verified: [how it connects to existing system]

**Technical Requirements:**
- [ ] Code follows project conventions: `npm run lint`
- [ ] Type safety maintained: `npm run typecheck` 
- [ ] Tests pass: `npm test`
- [ ] Performance acceptable: [specific metrics]

**Documentation Requirements:**
- [ ] Code is self-documenting
- [ ] Complex logic has comments
- [ ] Public APIs documented
- [ ] Breaking changes noted

**Validation Commands:**
```bash
# Specific commands to verify success
npm run build
npm test -- --testNamePattern="feature-name"
curl -X POST /api/endpoint -d '{"test":"data"}'
```
```

### Multiple Approach Evaluation
```markdown
## Approach Analysis for [Feature Name]

### Approach 1: [Name]
**Pros**: [Benefits and advantages]
**Cons**: [Drawbacks and risks]  
**Complexity**: Low/Medium/High
**Timeline**: [Estimated time]
**Risk Level**: Low/Medium/High

### Approach 2: [Name]
**Pros**: [Benefits and advantages]
**Cons**: [Drawbacks and risks]
**Complexity**: Low/Medium/High  
**Timeline**: [Estimated time]
**Risk Level**: Low/Medium/High

### Approach 3: [Name]
**Pros**: [Benefits and advantages]
**Cons**: [Drawbacks and risks]
**Complexity**: Low/Medium/High
**Timeline**: [Estimated time] 
**Risk Level**: Low/Medium/High

## Recommendation: [Chosen Approach]
**Reasoning**: [Why this approach is best given constraints]
**Fallback**: [If this doesn't work, what's plan B?]
```

## Todo File Generation

### Context-Rich Template Structure
```markdown
# Todo: [Feature/Fix Name]

## Context & Background
**Request**: [Original user request]
**Analysis Date**: [Date]
**Estimated Effort**: [Hours/Days]
**Risk Level**: HIGH/MEDIUM/LOW

## Task Sizing Assessment
**File Count**: [X files] - [Within target/Justified by atomicity]
**Estimated Time**: [X minutes] - [Target: 15-30min, Acceptable: up to 60min]
**Token Estimate**: [X tokens] - [Target: <150k, Acceptable: <300k]
**Complexity Level**: [1-3] - [Simple/Moderate/Complex]
**Atomicity Assessment**: ✅ ATOMIC - Cannot be meaningfully split further

### Codebase Context
**Existing Functionality**: 
- ✅ [What already works] - Files: [specific paths]
- ❌ [What's missing] - Location: [where it should go]
- ⚠️ [What's partially implemented] - Files: [specific paths]

**Similar Implementations**: 
- [File path] - [What pattern it uses] - [How it's relevant]
- [File path] - [What pattern it uses] - [How it's relevant]

**Dependencies**: 
- [Package name]@[version] - [Purpose] - [Documentation URL]
- [Package name]@[version] - [Purpose] - [Documentation URL]

**Architecture Integration**:
- [How this fits into existing system]
- [Components it will interact with] - Files: [paths]
- [Data flow/communication patterns]

### Task Scope Boundaries
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

### External Context Sources
**Primary Documentation**:
- [Title](URL) - [Key insights that influenced approach]
- [Title](URL) - [Key insights that influenced approach]

**Code References**:
- [Repo/Example](URL) - [Pattern demonstrated] 
- [Repo/Example](URL) - [Pattern demonstrated]

**Standards Applied**:
- [Standard](URL) - [Which parts are relevant]
- [Standard](URL) - [Which parts are relevant]

**Performance/Security Context**:
- Baseline: [Current metrics/security posture]
- Target: [Expected improvements/security enhancements]
- Constraints: [Limitations that affect implementation]

### User/Business Context
- **User Need**: [Why this is being built]
- **Success Criteria**: [How user will know it works]
- **Project Phase**: [Prototype/MVP/Production requirements]
- **Timeline**: [Any deadline constraints]

## Implementation Plan

### Phase 1: Foundation (Risk: [Level])
**Files**: [Exact file paths]
**Objective**: [Clear goal]
**Validation**: [How to verify success]

- [ ] Task 1: [Specific action]
  - **Risk**: [Level and reasoning]  
  - **Files**: [Exact paths]
  - **Success Criteria**: 
    - [ ] [Specific checkable outcome]
    - [ ] [Command to verify]: `specific command`
  - **Rollback**: [How to undo if needed]

### Phase 2: Core Implementation (Risk: [Level])
[Continue pattern...]

### Phase 3: Integration & Testing (Risk: [Level])
[Continue pattern...]

## Gotchas & Considerations
- **Known Issues**: [From investigation]
- **Edge Cases**: [Scenarios to test]
- **Performance**: [Expected impact]
- **Backwards Compatibility**: [What might break]
- **Security**: [Access control considerations]

## Definition of Done
- [ ] All tasks completed and verified
- [ ] Tests pass: `[specific test commands]`
- [ ] Performance acceptable: [metrics]
- [ ] Security review passed
- [ ] Documentation updated
- [ ] Code review completed (if applicable)
```

## Planning Output → Execution Handoff

### Create todo.md with this format:
```markdown
# Todo: [Feature/Fix Name]
**Generated from**: [Light/Full] Planning on [Date]
**Next Phase**: [tasks-execute.md](tasks-execute.md)

## Context Summary
- **Risk Level**: [Level] | **Project Phase**: [Phase] 
- **Estimated Effort**: [Time] | **Files**: [Count]
- **Feature Flag Required**: [Yes/No]

## Ready-to-Execute Tasks
[Tasks from planning with exact format needed for execution]

## Execution Notes
- **Start with**: [First task - usually highest risk]
- **Validation**: Run `[command]` after each task
- **Commit pattern**: `[component]: [action taken]`
```

## Approval Gate

### Pre-Implementation Checklist
```markdown
## Ready for Implementation? 
- [ ] High-risk items identified and mitigation planned
- [ ] File scope is reasonable (3-7 files max per task)
- [ ] Success criteria are specific and measurable  
- [ ] Validation commands are executable
- [ ] Dependencies are understood
- [ ] Rollback plan exists for high-risk changes
- [ ] User has reviewed and approved the plan
- [ ] todo.md created in execution-ready format

## Task Sizing Assessment
- [ ] **Time Assessment**: Tasks sized appropriately (target: 15-30min, max: 60min)
- [ ] **Token Assessment**: Token usage estimated (target: <150k, max: <300k)
- [ ] **Atomicity Verified**: Tasks cannot be meaningfully split further
- [ ] **Complexity Assessed**: All tasks categorized by complexity level

## Scope Boundary Validation
- [ ] **Usage Analysis**: Component/service usage analyzed for impact
- [ ] **Boundary Definition**: Clear MODIFY/REVIEW/IGNORE zones defined
- [ ] **No-Touch Areas**: Explicit ignore patterns documented
- [ ] **Impact Assessment**: Scope assessment completed (LIMITED/MODERATE/BROAD)
- [ ] **Focused Implementation**: Clear boundaries prevent unnecessary investigation

## Risk Communication
⚠️ **HIGH RISK ITEMS REQUIRING APPROVAL**:
[List any high-risk items with reasoning]

✅ **PROCEED**: Plan is comprehensive and risks are mitigated → Go to [tasks-execute.md](tasks-execute.md)
🛑 **PAUSE**: Get approval for high-risk items before implementation

## Handoff Complete
**Planning Phase Complete** → **Next: [tasks-execute.md](tasks-execute.md)**

### FILES CREATED - VERIFICATION REQUIRED ✅
**User must verify these files exist before proceeding:**

1. **Planning Output File**: `todo.md`
   - **Location**: Current working directory
   - **Contains**: Complete task breakdown with context
   - **Verify with**: `ls -la todo.md && head -20 todo.md`

2. **Context Package** (if full methodology used):
   - **Location**: `requests/<feature-name>/context/`
   - **Files**: `codebase-analysis.md`, `external-sources.md`, `architecture-decisions.md`
   - **Verify with**: `ls -la requests/*/context/ && find requests/ -name "*.md" -type f`

3. **Ready-to-Execute Tasks File**:
   - **Location**: Same directory as planning file
   - **Contains**: Tasks formatted for execution phase
   - **Verify with**: `grep -c "Task:" todo.md` (should show number of tasks)

**⚠️ CRITICAL**: The next agent (execution phase) will start with ZERO CONTEXT from this conversation. It will ONLY have access to:
- The files created above
- The current codebase state
- No memory of this planning discussion

**⚠️ STOP AND VERIFY**: User must confirm all files exist at specified locations before proceeding to execution phase.

**Next Step**: Only proceed to [tasks-execute.md](tasks-execute.md) after user confirms file creation.
```