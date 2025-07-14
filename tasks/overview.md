# Task Management System Overview

## The Four-Phase Methodology

### 📋 [tasks-plan.md](tasks-plan.md) - Planning Phase
**When to use**: Complex tasks, replacements, high-risk changes
**Time investment**: 5 minutes (light) to 30 minutes (full)
**Output**: todo.md file ready for execution

### ⚡ [tasks-execute.md](tasks-execute.md) - Implementation Phase  
**When to use**: All tasks (simple can start here directly)
**Focus**: Get work done efficiently, handle setbacks
**Output**: Working code, commits, updated todo.md

### ✅ [tasks-review.md](tasks-review.md) - Verification Phase
**When to use**: All completed tasks
**Time investment**: 5 minutes (quick) to 30 minutes (thorough)
**Output**: Quality verification, issue identification

### 🎯 [tasks-complete.md](tasks-complete.md) - Completion Phase
**When to use**: All verified tasks
**Time investment**: 5 minutes (simple) to 60 minutes (complex retrospective)
**Output**: Learning capture, methodology improvements

## Quick Decision Guide

### "I just want to start coding"
→ Go to [tasks-execute.md](tasks-execute.md) (Quick Start section)

### "This is risky/complex"
→ Start with [tasks-plan.md](tasks-plan.md) (Light or Full planning)

### "I'm replacing existing functionality"
→ **MANDATORY**: [tasks-plan.md](tasks-plan.md) (Full methodology + feature flags)

### "I'm in emergency/hotfix mode"
→ Go to [tasks-execute.md](tasks-execute.md) (Emergency Adaptations section)

### "This is just a prototype"
→ Use Light Planning + Prototype Speed Mode in execution

## Key Safety Rules

### 🚫 NEVER REMOVE old functionality without:
1. Feature flag implementation
2. Extended parallel operation (min 1 week production)
3. Proven new version superiority
4. Separate removal commit

### 📚 ALWAYS CAPTURE CONTEXT (Mandatory):
1. **Codebase Context**: Similar implementations, existing patterns, file paths
2. **External Context**: Documentation URLs, standards applied, dependency versions
3. **Architecture Context**: How change fits into existing system design
4. **Source Documentation**: Explicit links to all context sources used

### 📊 Scale methodology by project phase:
- **Prototype**: Speed > perfection, recreate data vs migrate, but still capture context
- **MVP/Beta**: User validation, basic production readiness, comprehensive context
- **Production**: Full security, monitoring, cross-team coordination, complete context

### 🔄 Always capture learnings:
Each completion phase feeds back to improve future planning/execution

## Cross-Document Navigation

```
Planning → Execution → Review → Completion
    ↓         ↓         ↓         ↓
  todo.md   commits   issues   learnings
    ↓         ↓         ↓         ↓
Archive with complete knowledge trail
```

## Common Workflows

### Simple Feature Addition:
1. [tasks-execute.md](tasks-execute.md) (Quick Start)
   - **Context**: Still identify similar patterns and key files
2. [tasks-review.md](tasks-review.md) (5-minute check + context validation)  
3. [tasks-complete.md](tasks-complete.md) (Quick completion)

### Complex New Feature:
1. [tasks-plan.md](tasks-plan.md) (Full methodology + comprehensive context capture)
   - **Mandatory**: Codebase investigation, external research, source documentation
2. [tasks-execute.md](tasks-execute.md) (Planned tasks + context-informed implementation)
3. [tasks-review.md](tasks-review.md) (Full review + context application assessment)
4. [tasks-complete.md](tasks-complete.md) (Full retrospective + context learning capture)

### Replacing Existing System:
1. [tasks-plan.md](tasks-plan.md) (Mandatory full planning)
2. [tasks-execute.md](tasks-execute.md) (Feature flag example)
3. [tasks-review.md](tasks-review.md) (Both systems validation)
4. [tasks-complete.md](tasks-complete.md) (Parallel operation verification)

### Emergency/Hotfix:
1. [tasks-execute.md](tasks-execute.md) (Emergency mode)
2. [tasks-review.md](tasks-review.md) (Critical checks only)
3. [tasks-complete.md](tasks-complete.md) (Document shortcuts taken)

## Integration with /tasks Command

This methodology is designed to work with the `/tasks` command execution:
- **Planning output** creates execution-ready todo.md
- **Execution phase** provides uncluttered action focus
- **Review/completion** capture learnings for future improvement

The goal is practical task management that scales with complexity while maintaining development velocity.