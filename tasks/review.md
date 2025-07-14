# Task Review & Verification

## Quick Review (5-minute version)

### Essential Checks Only:
```bash
# Run these commands
npm run lint && npm run typecheck && npm test

# Manual check
- [ ] Original user request is satisfied
- [ ] No critical bugs introduced
- [ ] Performance acceptable for current project phase
- [ ] Feature flag implemented (if replacing functionality)
- [ ] Context sources were referenced during implementation
- [ ] Similar patterns in codebase were followed
```

**Simple tasks**: If all pass → Go to [tasks-complete.md](tasks-complete.md)
**Complex tasks**: Continue with full review below

---

## Pre-Review Checklist (Full Methodology)

### Code Quality Verification
```bash
# Automated quality checks
npm run lint                # Code style compliance
npm run typecheck          # Type safety verification  
npm run test              # Unit test execution
npm run build             # Build process verification
npm audit                 # Security vulnerability check

# Test coverage verification
npm run test -- --coverage
# Target: >80% coverage for new code
```

### Functional Verification
```markdown
## Success Criteria Validation
- [ ] Each success criterion checked individually
- [ ] Validation commands executed successfully
- [ ] Expected outputs match actual outputs
- [ ] Edge cases tested and handled appropriately
- [ ] Error scenarios verified and properly handled
```

## Review Methodology

### 1. Requirements Verification
```markdown
## Original Request Compliance
**Original Request**: [Copy exact user request]
**Delivered Solution**: [What was actually implemented]

### Requirement Analysis
- ✅ **Core Requirement Met**: [How core need is satisfied]
- ✅ **Acceptance Criteria**: [All criteria verified]
- ⚠️ **Scope Changes**: [Any deviations and reasons]
- ❌ **Missing Elements**: [What wasn't completed and why]

### Success Metrics
- **Functional**: [Feature works as specified]
- **Technical**: [Code quality meets standards]  
- **Performance**: [Meets performance requirements]
- **Security**: [No security issues introduced]
- **Maintainability**: [Code is maintainable]
```

### 2. Technical Implementation Review
```markdown
## Code Quality Assessment

### Context Application Review
- [ ] Implementation follows patterns identified in similar code
- [ ] External standards/documentation were properly applied
- [ ] Dependencies are used according to their documentation
- [ ] Architecture decisions align with project context
- [ ] Performance/security constraints from context were addressed

### Architecture Alignment
- [ ] Follows existing patterns and conventions
- [ ] Integrates properly with existing codebase
- [ ] Uses appropriate design patterns
- [ ] Maintains separation of concerns
- [ ] Consistent with similar implementations found during planning

### Code Craftsmanship
- [ ] Code is readable and self-documenting
- [ ] Complex logic has appropriate comments
- [ ] Functions are single-purpose and appropriately sized
- [ ] Error handling is comprehensive
- [ ] Resource management is proper (memory, connections, etc.)

### Testing Coverage
- [ ] Unit tests cover core functionality
- [ ] Integration tests verify component interaction
- [ ] Edge cases are tested
- [ ] Error scenarios are tested
- [ ] Test code is maintainable and clear
```

### 3. Integration Verification
```markdown
## System Integration Check

### Backwards Compatibility
- [ ] Existing functionality unaffected
- [ ] API contracts maintained
- [ ] Database migrations are reversible
- [ ] Configuration changes are backwards compatible

### Cross-System Impact
- [ ] Dependent systems function correctly
- [ ] External integrations work properly
- [ ] Performance impact is acceptable
- [ ] Security boundaries are maintained
```

## Evaluation Criteria

### Quality Dimensions

#### 1. Functional Quality (Weight: 30%)
```markdown
**Excellent (5/5)**:
- All requirements fully implemented
- Edge cases handled comprehensively
- User experience is smooth and intuitive
- Error messages are helpful and actionable

**Good (4/5)**:
- Core requirements implemented
- Most edge cases handled
- Minor UX issues that don't affect core functionality
- Error handling covers expected scenarios

**Acceptable (3/5)**:
- Basic requirements met
- Some edge cases handled
- Functional but may have usability issues
- Basic error handling in place

**Poor (2/5)**:
- Requirements partially met
- Limited edge case handling
- Usability issues affect core functionality
- Minimal error handling

**Failing (1/5)**:
- Core requirements not met
- No edge case handling
- Major functionality issues
- No proper error handling
```

#### 2. Technical Quality (Weight: 25%)
```markdown
**Excellent (5/5)**:
- Code follows all project conventions
- Excellent separation of concerns
- Comprehensive test coverage (>90%)
- Performance optimized
- No technical debt introduced

**Good (4/5)**:
- Minor deviations from conventions
- Good architecture with clear structure
- Good test coverage (>80%)
- Performance acceptable
- Minimal technical debt

**Acceptable (3/5)**:
- Some convention violations
- Adequate architecture
- Basic test coverage (>60%)
- Performance meets minimum requirements
- Some technical debt acceptable

**Poor (2/5)**:
- Multiple convention violations
- Poor architecture choices
- Insufficient test coverage (<60%)
- Performance issues
- Significant technical debt

**Failing (1/5)**:
- Major convention violations
- No clear architecture
- No meaningful test coverage
- Unacceptable performance
- Creates maintenance burden
```

#### 3. Security Quality (Weight: 20%)
```markdown
**Excellent (5/5)**:
- Security best practices followed
- Input validation comprehensive
- Authentication/authorization proper
- No sensitive data exposure
- Security tests included

**Good (4/5)**:
- Most security practices followed
- Good input validation
- Minor security considerations overlooked
- Sensitive data handled properly

**Acceptable (3/5)**:
- Basic security measures in place
- Adequate input validation
- No major security flaws
- Room for security improvements

**Poor (2/5)**:
- Some security measures missing
- Inadequate input validation
- Potential security vulnerabilities
- Security not prioritized

**Failing (1/5)**:
- Major security flaws
- No input validation
- Authentication/authorization broken
- Sensitive data exposed
```

#### 4. Maintainability (Weight: 15%)
```markdown
**Excellent (5/5)**:
- Code is self-documenting
- Clear, logical structure
- Easy to extend and modify
- Comprehensive documentation
- Future-proof design

**Good (4/5)**:
- Generally well-structured
- Good naming conventions
- Some documentation present
- Relatively easy to modify

**Acceptable (3/5)**:
- Adequate structure
- Reasonable naming
- Basic documentation
- Moderately maintainable

**Poor (2/5)**:
- Poor structure
- Unclear naming
- Little documentation
- Difficult to maintain

**Failing (1/5)**:
- No clear structure
- Confusing implementation
- No documentation
- Unmaintainable
```

#### 5. Performance (Weight: 10%)
```markdown
**Excellent (5/5)**:
- Exceeds performance requirements
- Optimized algorithms and data structures
- Efficient resource usage
- Scalable implementation

**Good (4/5)**:
- Meets performance requirements
- Generally efficient implementation
- Good resource management
- Reasonable scalability

**Acceptable (3/5)**:
- Meets minimum performance requirements
- Basic efficiency considerations
- Adequate resource usage

**Poor (2/5)**:
- Below performance requirements
- Inefficient implementation
- Poor resource management

**Failing (1/5)**:
- Unacceptable performance
- No performance considerations
- Resource waste
```

## Review Process

### 1. Automated Verification
```bash
#!/bin/bash
# Automated review script

echo "Running automated quality checks..."

# Code quality
npm run lint || exit 1
npm run typecheck || exit 1

# Test execution
npm test || exit 1

# Security check
npm audit || echo "Security audit warnings present"

# Build verification  
npm run build || exit 1

# Performance baseline (if applicable)
npm run perf-test || echo "Performance tests not available"

echo "Automated checks completed successfully"
```

### 2. Manual Review Steps
```markdown
## Manual Review Checklist

### Code Review
- [ ] Read through all changed files
- [ ] Verify logic is correct and efficient
- [ ] Check for potential bugs or edge cases
- [ ] Ensure error handling is appropriate
- [ ] Validate security considerations

### Integration Review
- [ ] Test manual user scenarios
- [ ] Verify API endpoints work correctly
- [ ] Check database operations
- [ ] Validate external service integration
- [ ] Test error scenarios manually

### Documentation Review
- [ ] Code comments are helpful and accurate
- [ ] Public API is documented
- [ ] Breaking changes are noted
- [ ] README updates (if applicable)
```

### 3. Stakeholder Review
```markdown
## User Acceptance Testing
- [ ] User can complete primary use case
- [ ] User experience meets expectations
- [ ] Error messages are understandable
- [ ] Performance is acceptable to user
- [ ] Integration with existing workflow is smooth

## Technical Review (if applicable)
- [ ] Senior developer code review
- [ ] Architecture review for significant changes
- [ ] Security review for sensitive changes
- [ ] Performance review for critical paths
```

## Issue Classification

### Bug Severity Levels
```markdown
## CRITICAL (Must Fix Before Approval)
- Core functionality broken
- Data corruption possible
- Security vulnerabilities
- System crashes/instability

## HIGH (Should Fix Before Approval)  
- Important features not working
- Poor user experience
- Performance significantly below requirements
- Major edge cases not handled

## MEDIUM (Fix if Time Allows)
- Minor feature issues
- Small usability problems
- Performance slightly below optimal
- Non-critical edge cases

## LOW (Future Enhancement)
- Nice-to-have improvements
- Minor optimization opportunities
- Cosmetic issues
- Documentation improvements
```

### Resolution Tracking
```markdown
## Issue: [Description]
**Severity**: Critical/High/Medium/Low
**Impact**: [How it affects the system/user]
**Root Cause**: [Why it happened]
**Resolution**: [How it was fixed]
**Prevention**: [How to avoid in future]
**Status**: Open/In Progress/Resolved/Deferred
```

## Review Documentation

### Review Report Template
```markdown
# Task Review Report

## Task Summary
**Task**: [Task name and description]
**Date Completed**: [Date]
**Reviewer**: [Name/Role]
**Review Date**: [Date]

## Overall Assessment
**Status**: ✅ Approved / ⚠️ Approved with Conditions / ❌ Rejected
**Quality Score**: [X/5] (see scoring breakdown below)

## Scoring Breakdown
- **Functional Quality**: [X/5] - [Brief reasoning]
- **Technical Quality**: [X/5] - [Brief reasoning]  
- **Security Quality**: [X/5] - [Brief reasoning]
- **Maintainability**: [X/5] - [Brief reasoning]
- **Performance**: [X/5] - [Brief reasoning]

## What Went Well
- [Positive aspects of the implementation]
- [Good practices followed]
- [Exceeded expectations in areas]
- [Context sources that were particularly valuable]
- [Effective use of similar patterns from codebase]

## Issues Found
### Critical Issues
- [List critical issues that must be fixed]

### High Priority Issues  
- [List high priority issues]

### Medium/Low Priority Issues
- [List minor issues for future consideration]

### Context/Research Issues
- [Missing context that would have improved implementation]
- [External sources that should have been consulted]
- [Similar code patterns that were overlooked]

## Recommendations
- [Suggestions for improvement]
- [Best practices for future tasks]
- [Process improvements]
- [Additional context sources for similar future tasks]
- [Patterns/standards to document for reuse]

## Sign-off
- [ ] All critical issues resolved
- [ ] Documentation updated
- [ ] Stakeholder approval obtained
- [ ] Ready for production/next phase

**Reviewer Signature**: [Name, Date]

## Review Complete → Next Phase

### Simple Tasks:
**Review Passed** → Go to [tasks-complete.md](tasks-complete.md) (Quick Completion)

### Complex Tasks:
**Review Passed** → Go to [tasks-complete.md](tasks-complete.md) (Full Retrospective)

### Issues Found:
**Critical Issues** → Return to [tasks-execute.md](tasks-execute.md) to fix
**Minor Issues** → Document for future improvement, proceed to completion
```