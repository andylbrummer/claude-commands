# Bug Analysis Prompt

Analyze the following bug report or error log:

$ARGUMENTS

## Analysis Steps:

1. **Problem Identification**: Carefully examine the error message, stack trace, or bug description to identify the best places to look for the root cause. 
   - Question your assumptions!
   - Look for keywords that indicate the nature of the bug (e.g., "NullPointerException", "404 Not Found", "TypeError").
   - Identify the file and line number where the error occurs, if available.
   - Determine if this is a new bug or a regression by checking recent changes in the codebase.
   - If applicable, check for any related issues in the issue tracker or project documentation.
   - If there is any ambiguity think about multiple options and investigate them from most likely to least likely leaving no stone unturned until you rule out all the options or find the root cause.


2. **Context Analysis**: Consider the system context, affected components, and potential side effects. Check the project documentation for the location of additional debugging information like tests, logs, or related issues. 
   - Look for relevant files in the codebase that may provide insight into the bug.
   - Run any related tests, and try to reproduce the issue locally if the issue isn't obvious.

3. **Generate Solutions**: Think through up to 5 potential fixes, considering:
   - Root cause resolution
   - Code maintainability
   - Performance impact
   - Potential side effects
   - Implementation complexity

4. **Rank Solutions**: Evaluate and rank the top 3 solutions from best to worst based on:
   - Effectiveness in solving the problem
   - Safety and reliability
   - Implementation effort
   - Long-term maintainability

## Deliverables:

Present the top 3 solutions in order from best to worst:

### Solution 1 (Recommended):
- **Fix**: [Concise description]
- **Implementation**: [Key steps]
- **Why this works**: [Brief explanation]
- **Pros/Cons**: [Quick assessment]

### Solution 2 (Alternative):
- **Fix**: [Concise description]
- **Implementation**: [Key steps]
- **Why this works**: [Brief explanation]
- **Pros/Cons**: [Quick assessment]

### Solution 3 (Fallback):
- **Fix**: [Concise description]
- **Implementation**: [Key steps]
- **Why this works**: [Brief explanation]
- **Pros/Cons**: [Quick assessment]

**Which solution would you like me to implement?**