# Simple Analysis Command

Analyzes the provided arguments and current project context to provide a comprehensive understanding before starting work.

## Usage
```
/simple [arguments or request description]
```

## What it does
Takes the provided arguments and analyzes them against the current project context to:

1. **Understand the Request**: Parse and interpret what is being asked
2. **Identify Edge Cases**: Consider potential complications or special scenarios
3. **Create Implementation Plan**: Outline approach including:
   - Files that will be impacted
   - Areas to avoid or be careful with
   - Potential impact to the user
4. **Simple Design**: Provide basic architectural or design considerations
5. **Confirm Understanding**: Present analysis for validation before proceeding

## Implementation

Analyze the request: $ARGUMENTS

Based on the current project context, I'll provide:

### Understanding of Request
[What exactly is being asked for]

### Edge Cases Identified
[Potential complications or special scenarios to consider]

### Implementation Plan
**Files to Impact:**
- [List of files that will be modified/created]

**Areas to Avoid:**
- [Sensitive areas or constraints to respect]

**User Impact:**
- [How this change affects the user experience]

### Simple Design
[Basic architectural approach or design pattern to follow]

### Confirmation
Does this understanding align with your expectations? Should I proceed with this approach?