# Subtasks Status Reporting & Archive Summary
## Large Feature Development Status Tracking

## Quick Status Overview

### Current Large Features Status Check
```bash
# Generate immediate large feature status overview
echo "=== LARGE FEATURES STATUS OVERVIEW ==="
echo "Generated on: $(date)"
echo

# Check for active large feature contexts
echo "Active Large Feature Contexts:"
find . -name "context" -type d 2>/dev/null | while read context_dir; do
    if [[ -d "$context_dir" ]]; then
        feature_name=$(dirname "$context_dir" | xargs basename)
        echo "🏗️ Feature: $feature_name"
        echo "   Context: $context_dir"
        
        # Check context completeness
        local context_files=("codebase-analysis.md" "external-sources.md" "implementation-patterns.md" "architecture-decisions.md")
        local present_count=0
        for file in "${context_files[@]}"; do
            if [[ -f "$context_dir/$file" ]]; then
                present_count=$((present_count + 1))
            fi
        done
        echo "   Context Completeness: $present_count/${#context_files[@]} files"
    fi
done

# Check for worktree structures
echo
echo "Active Worktree Structures:"
if [[ -d "~/work/worktrees" ]]; then
    find ~/work/worktrees -mindepth 1 -maxdepth 1 -type d 2>/dev/null | while read feature_dir; do
        feature_name=$(basename "$feature_dir")
        task_count=$(find "$feature_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
        echo "🌳 Feature: $feature_name ($task_count subtasks)"
        
        # Check subtask status
        find "$feature_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | while read task_dir; do
            task_name=$(basename "$task_dir")
            if [[ -f "$task_dir/completion_report.md" ]]; then
                echo "   ✅ $task_name: Complete"
            elif [[ -f "$task_dir/status.txt" ]]; then
                status=$(cat "$task_dir/status.txt" 2>/dev/null || echo "Unknown")
                echo "   🔄 $task_name: $status"
            else
                echo "   ⏳ $task_name: Status unknown"
            fi
        done
    done
else
    echo "No worktree structures found at ~/work/worktrees"
fi

# Check for active subtask todo files
echo
echo "Active Subtask Todo Files:"
find . -path "*/tasks/*.md" -o -path "*/requests/*/tasks/*.md" 2>/dev/null | while read file; do
    if grep -q "^- \[ \]\|^- \[x\]" "$file" 2>/dev/null; then
        echo "📋 Found: $file"
        incomplete=$(grep -c "^- \[ \]" "$file" 2>/dev/null || echo "0")
        complete=$(grep -c "^- \[x\]" "$file" 2>/dev/null || echo "0")
        echo "   Subtasks: $complete completed, $incomplete remaining"
    fi
done

echo
echo "=== END LARGE FEATURES STATUS OVERVIEW ==="
```

## Comprehensive Large Feature Status Report

### MANDATORY: Large Feature Analysis
```markdown
## Large Feature Status Analysis Template

### Feature Discovery and Context Analysis
- **Search Locations**: Context packages, worktree structures, request folders
- **Context Completeness**: Verify all required context files present
- **Subagent Coordination**: Check for active subagent work and coordination
- **Integration Status**: Verify cross-task integration points

### Cross-Task Status Analysis
```bash
# Comprehensive large feature status analysis
generate_large_feature_status_report() {
    local report_file="large-feature-status-report-$(date +%Y%m%d-%H%M).md"
    
    cat > "$report_file" << 'EOF'
# Large Feature Status Report
**Generated**: $(date)
**Analysis Scope**: Large features using subtask methodology

## Executive Summary
EOF

    echo "## Executive Summary" >> "$report_file"
    
    # Count active large features
    local feature_contexts=$(find . -name "context" -type d 2>/dev/null | wc -l)
    local worktree_features=$(find ~/work/worktrees -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
    
    echo "- **Active Feature Contexts**: $feature_contexts" >> "$report_file"
    echo "- **Worktree Features**: $worktree_features" >> "$report_file"
    
    # Analyze each large feature
    echo "" >> "$report_file"
    echo "## Active Large Features Analysis" >> "$report_file"
    
    # Check context-based features
    find . -name "context" -type d 2>/dev/null | while read context_dir; do
        local feature_path=$(dirname "$context_dir")
        local feature_name=$(basename "$feature_path")
        
        echo "" >> "$report_file"
        echo "### Feature: $feature_name" >> "$report_file"
        echo "**Context Location**: $context_dir" >> "$report_file"
        
        # Context completeness analysis
        echo "**Context Analysis**:" >> "$report_file"
        local context_files=("codebase-analysis.md" "external-sources.md" "implementation-patterns.md" "architecture-decisions.md")
        local present_count=0
        
        for file in "${context_files[@]}"; do
            if [[ -f "$context_dir/$file" ]]; then
                present_count=$((present_count + 1))
                # Get file size and last modified
                local size=$(stat -f%z "$context_dir/$file" 2>/dev/null || stat -c%s "$context_dir/$file" 2>/dev/null || echo "0")
                local modified=$(stat -f%Sm "$context_dir/$file" 2>/dev/null || stat -c%y "$context_dir/$file" 2>/dev/null | cut -d' ' -f1)
                echo "- ✅ $file (${size} bytes, modified: $modified)" >> "$report_file"
            else
                echo "- ❌ $file (missing)" >> "$report_file"
            fi
        done
        
        local completeness_percent=$((present_count * 100 / ${#context_files[@]}))
        echo "**Context Completeness**: $present_count/${#context_files[@]} ($completeness_percent%)" >> "$report_file"
        
        # Check for task files
        local task_dir="$feature_path/tasks"
        if [[ -d "$task_dir" ]]; then
            local task_count=$(find "$task_dir" -name "*.md" 2>/dev/null | wc -l)
            echo "**Task Files**: $task_count found in $task_dir" >> "$report_file"
            
            # Analyze task completion
            local total_subtasks=0
            local completed_subtasks=0
            
            find "$task_dir" -name "*.md" 2>/dev/null | while read task_file; do
                local file_total=$(grep -c "^- \[ \]\|^- \[x\]" "$task_file" 2>/dev/null)
                local file_completed=$(grep -c "^- \[x\]" "$task_file" 2>/dev/null)
                total_subtasks=$((total_subtasks + file_total))
                completed_subtasks=$((completed_subtasks + file_completed))
            done
            
            if [[ $total_subtasks -gt 0 ]]; then
                local completion_rate=$((completed_subtasks * 100 / total_subtasks))
                echo "**Subtask Progress**: $completed_subtasks/$total_subtasks ($completion_rate%)" >> "$report_file"
            fi
        else
            echo "**Task Files**: No task directory found" >> "$report_file"
        fi
        
        # Check for worktree coordination
        local worktree_path="~/work/worktrees/$feature_name"
        if [[ -d "$worktree_path" ]]; then
            echo "**Worktree Status**: Active at $worktree_path" >> "$report_file"
            
            # Analyze individual subtask worktrees
            local subtask_dirs=$(find "$worktree_path" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
            echo "**Parallel Subtasks**: $subtask_dirs active" >> "$report_file"
            
            # Check subtask status
            find "$worktree_path" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | while read subtask_dir; do
                local subtask_name=$(basename "$subtask_dir")
                
                if [[ -f "$subtask_dir/completion_report.md" ]]; then
                    echo "- ✅ $subtask_name: Complete" >> "$report_file"
                elif [[ -f "$subtask_dir/status.txt" ]]; then
                    local status=$(cat "$subtask_dir/status.txt" | head -1)
                    echo "- 🔄 $subtask_name: $status" >> "$report_file"
                else
                    echo "- ⏳ $subtask_name: Status unknown" >> "$report_file"
                fi
            done
        else
            echo "**Worktree Status**: No active worktree found" >> "$report_file"
        fi
        
        # Feature health assessment
        echo "**Health Assessment**:" >> "$report_file"
        if [[ $completeness_percent -ge 80 ]] && [[ -d "$task_dir" ]]; then
            echo "- 🟢 **Good**: Context complete, tasks defined" >> "$report_file"
        elif [[ $completeness_percent -ge 60 ]]; then
            echo "- 🟡 **Fair**: Context mostly complete, may need attention" >> "$report_file"
        else
            echo "- 🔴 **Poor**: Context incomplete, requires immediate attention" >> "$report_file"
        fi
    done
    
    # Cross-feature analysis
    echo "" >> "$report_file"
    echo "## Cross-Feature Analysis" >> "$report_file"
    
    # Resource usage analysis
    echo "**Resource Usage**:" >> "$report_file"
    local active_worktrees=$(find ~/work/worktrees -mindepth 2 -maxdepth 2 -type d 2>/dev/null | wc -l)
    echo "- **Active Worktrees**: $active_worktrees" >> "$report_file"
    
    # Integration dependencies
    echo "**Integration Dependencies**:" >> "$report_file"
    if find . -name "*.md" -exec grep -l "depends.*on\|blocked.*by\|requires.*completion" {} \; 2>/dev/null | grep -q .; then
        echo "- ⚠️ **Dependencies Found**: Some features have cross-dependencies" >> "$report_file"
        find . -name "*.md" -exec grep -H "depends.*on\|blocked.*by\|requires.*completion" {} \; 2>/dev/null | head -5 | sed 's/^/  /' >> "$report_file"
    else
        echo "- ✅ **No Dependencies**: Features appear independent" >> "$report_file"
    fi
    
    # Recommendations
    echo "" >> "$report_file"
    echo "## Recommendations" >> "$report_file"
    
    if [[ $feature_contexts -eq 0 ]]; then
        echo "- 📋 **No large features active** - Consider using subtask methodology for complex features" >> "$report_file"
    elif [[ $feature_contexts -eq 1 ]]; then
        echo "- 🎯 **Single feature focus** - Good for maintaining context and reducing complexity" >> "$report_file"
    else
        echo "- ⚠️ **Multiple large features** - Monitor resource usage and context switching overhead" >> "$report_file"
    fi
    
    echo "**Large feature report saved to**: $report_file"
    echo "**View with**: cat $report_file"
}

# Execute the function
generate_large_feature_status_report
```

### Subagent Coordination Status
```bash
# Monitor subagent coordination and parallel development
analyze_subagent_coordination() {
    echo "=== SUBAGENT COORDINATION ANALYSIS ==="
    
    # Check for active worktree coordination
    for feature_dir in ~/work/worktrees/*/; do
        if [[ -d "$feature_dir" ]]; then
            local feature_name=$(basename "$feature_dir")
            echo "🌳 Feature: $feature_name"
            
            # Check for coordination files
            if [[ -f "$feature_dir/coordination.md" ]]; then
                echo "  ✅ Coordination documentation present"
            else
                echo "  ⚠️ No coordination documentation"
            fi
            
            # Check for file conflicts
            local conflict_count=0
            find "$feature_dir" -mindepth 1 -maxdepth 1 -type d | while read subtask_dir; do
                if git -C "$subtask_dir" status 2>/dev/null | grep -q "both modified\|both added"; then
                    echo "  🔥 MERGE CONFLICT in $(basename "$subtask_dir")"
                    conflict_count=$((conflict_count + 1))
                fi
            done
            
            # Check for status reporting
            local reporting_subtasks=0
            local total_subtasks=0
            find "$feature_dir" -mindepth 1 -maxdepth 1 -type d | while read subtask_dir; do
                total_subtasks=$((total_subtasks + 1))
                if [[ -f "$subtask_dir/status.txt" ]]; then
                    reporting_subtasks=$((reporting_subtasks + 1))
                fi
            done
            
            if [[ $total_subtasks -gt 0 ]]; then
                local reporting_rate=$((reporting_subtasks * 100 / total_subtasks))
                echo "  📊 Status Reporting: $reporting_subtasks/$total_subtasks ($reporting_rate%)"
            fi
        fi
    done
    
    # Check for context synchronization
    echo
    echo "🔄 Context Synchronization:"
    find . -name "context" -type d | while read context_dir; do
        local feature_name=$(dirname "$context_dir" | xargs basename)
        
        # Check if context is linked to worktrees
        local worktree_path="~/work/worktrees/$feature_name"
        if [[ -d "$worktree_path" ]]; then
            local linked_count=0
            local total_subtasks=0
            
            find "$worktree_path" -mindepth 1 -maxdepth 1 -type d | while read subtask_dir; do
                total_subtasks=$((total_subtasks + 1))
                if [[ -L "$subtask_dir/context" ]] || [[ -d "$subtask_dir/context" ]]; then
                    linked_count=$((linked_count + 1))
                fi
            done
            
            echo "  $feature_name: $linked_count/$total_subtasks subtasks have context access"
        fi
    done
}

analyze_subagent_coordination
```

## Archive Summary Report (Large Features)

### MANDATORY: Large Feature Archive Analysis
```bash
# Generate comprehensive large feature archive summary
generate_large_feature_archive_summary() {
    local archive_report="large-feature-archive-summary-$(date +%Y%m%d-%H%M).md"
    
    cat > "$archive_report" << 'EOF'
# Large Feature Archive Summary Report
**Generated**: $(date)
**Focus**: Large features using subtask methodology

## Large Feature Archive Overview
EOF

    echo "## Large Feature Archive Overview" >> "$archive_report"
    
    # Find feature archives
    local archive_dirs="/archive/features ./archive/features ../archive/features ~/archive/features /home/beagle/.claude/archive/features"
    local found_archives=""
    
    for dir in $archive_dirs; do
        if [[ -d "$dir" ]]; then
            found_archives="$found_archives $dir"
        fi
    done
    
    if [[ -z "$found_archives" ]]; then
        echo "❌ **No large feature archives found**" >> "$archive_report"
        echo "- Searched locations: /archive/features, ./archive/features, etc." >> "$archive_report"
        echo "- Large features should be archived in feature-specific directories" >> "$archive_report"
    else
        echo "📁 **Feature Archive Directories**: $found_archives" >> "$archive_report"
        
        # Analyze each feature archive directory
        for archive_dir in $found_archives; do
            echo "" >> "$archive_report"
            echo "### Feature Archive: $archive_dir" >> "$archive_report"
            
            local total_features=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
            echo "**Total Archived Features**: $total_features" >> "$archive_report"
            
            # Recent features (last 60 days - longer timeframe for large features)
            local recent_count=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -newermt "$(date -d '60 days ago' '+%Y-%m-%d')" 2>/dev/null | wc -l)
            echo "**Recent Features (60 days)**: $recent_count" >> "$archive_report"
            
            # List recent archived features with detailed analysis
            echo "**Recent Archived Features**:" >> "$archive_report"
            find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -printf '%T@ %p\n' 2>/dev/null | \
            sort -rn | head -5 | while read timestamp path; do
                local date_str=$(date -d "@$timestamp" '+%Y-%m-%d' 2>/dev/null || echo "unknown")
                local feature_name=$(basename "$path")
                echo "- [$date_str] **$feature_name**" >> "$archive_report"
                
                # Check for comprehensive large feature documentation
                local has_context=$(find "$path" -name "context" -type d | wc -l)
                local has_tasks=$(find "$path" -name "tasks" -type d | wc -l)
                local has_retrospective=$(find "$path" -name "*retrospective*" | wc -l)
                local has_integration_review=$(find "$path" -name "*integration*" | wc -l)
                
                if [[ $has_context -gt 0 ]]; then
                    echo "  ✅ Context package archived" >> "$archive_report"
                fi
                if [[ $has_tasks -gt 0 ]]; then
                    local task_count=$(find "$path/tasks" -name "*.md" 2>/dev/null | wc -l)
                    echo "  ✅ Task files archived ($task_count tasks)" >> "$archive_report"
                fi
                if [[ $has_retrospective -gt 0 ]]; then
                    echo "  ✅ Retrospective documentation present" >> "$archive_report"
                fi
                if [[ $has_integration_review -gt 0 ]]; then
                    echo "  ✅ Integration review documentation present" >> "$archive_report"
                fi
                
                # Check for worktree bundles
                local bundle_count=$(find "$path" -name "*.bundle" 2>/dev/null | wc -l)
                if [[ $bundle_count -gt 0 ]]; then
                    echo "  ✅ Worktree bundles archived ($bundle_count bundles)" >> "$archive_report"
                fi
                
                # Calculate archive completeness score
                local completeness_score=0
                local max_score=5
                [[ $has_context -gt 0 ]] && completeness_score=$((completeness_score + 1))
                [[ $has_tasks -gt 0 ]] && completeness_score=$((completeness_score + 1))
                [[ $has_retrospective -gt 0 ]] && completeness_score=$((completeness_score + 1))
                [[ $has_integration_review -gt 0 ]] && completeness_score=$((completeness_score + 1))
                [[ $bundle_count -gt 0 ]] && completeness_score=$((completeness_score + 1))
                
                local completeness_percent=$((completeness_score * 100 / max_score))
                echo "  📊 Archive Completeness: $completeness_score/$max_score ($completeness_percent%)" >> "$archive_report"
            done
            
            # Feature type analysis
            echo "" >> "$archive_report"
            echo "**Feature Type Analysis**:" >> "$archive_report"
            
            # Analyze by complexity (number of subtasks)
            local simple_features=0
            local complex_features=0
            local very_complex_features=0
            
            find "$archive_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | while read feature_path; do
                local task_count=0
                if [[ -d "$feature_path/tasks" ]]; then
                    task_count=$(find "$feature_path/tasks" -name "*.md" 2>/dev/null | wc -l)
                fi
                
                if [[ $task_count -le 3 ]]; then
                    simple_features=$((simple_features + 1))
                elif [[ $task_count -le 8 ]]; then
                    complex_features=$((complex_features + 1))
                else
                    very_complex_features=$((very_complex_features + 1))
                fi
            done
            
            echo "- **Simple Features** (≤3 subtasks): $simple_features" >> "$archive_report"
            echo "- **Complex Features** (4-8 subtasks): $complex_features" >> "$archive_report"
            echo "- **Very Complex Features** (>8 subtasks): $very_complex_features" >> "$archive_report"
        done
    fi
    
    # Cross-feature insights
    echo "" >> "$archive_report"
    echo "## Large Feature Development Insights" >> "$archive_report"
    
    if [[ -n "$found_archives" ]]; then
        echo "### Context Methodology Effectiveness" >> "$archive_report"
        
        # Analyze context package quality across archived features
        local features_with_context=0
        local total_archived_features=0
        
        for archive_dir in $found_archives; do
            local dir_feature_count=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
            total_archived_features=$((total_archived_features + dir_feature_count))
            
            local dir_context_count=$(find "$archive_dir" -mindepth 2 -maxdepth 2 -name "context" -type d 2>/dev/null | wc -l)
            features_with_context=$((features_with_context + dir_context_count))
        done
        
        if [[ $total_archived_features -gt 0 ]]; then
            local context_adoption_rate=$((features_with_context * 100 / total_archived_features))
            echo "- **Context Package Adoption**: $features_with_context/$total_archived_features features ($context_adoption_rate%)" >> "$archive_report"
        fi
        
        echo "### Subagent Coordination Patterns" >> "$archive_report"
        
        # Look for patterns in archived features
        local features_with_worktrees=0
        for archive_dir in $found_archives; do
            local worktree_features=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -exec find {} -name "*.bundle" \; 2>/dev/null | wc -l)
            features_with_worktrees=$((features_with_worktrees + worktree_features))
        done
        
        if [[ $total_archived_features -gt 0 ]]; then
            local worktree_usage_rate=$((features_with_worktrees * 100 / total_archived_features))
            echo "- **Worktree Coordination Usage**: $features_with_worktrees/$total_archived_features features ($worktree_usage_rate%)" >> "$archive_report"
        fi
        
        echo "### Development Velocity Analysis" >> "$archive_report"
        
        # Monthly large feature completion rate
        local current_month=$(date '+%Y-%m')
        local this_month_features=0
        local last_month_features=0
        
        for archive_dir in $found_archives; do
            this_month_features=$((this_month_features + $(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -name "${current_month}*" 2>/dev/null | wc -l)))
            local last_month=$(date -d 'last month' '+%Y-%m')
            last_month_features=$((last_month_features + $(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -name "${last_month}*" 2>/dev/null | wc -l)))
        done
        
        echo "- **This Month**: $this_month_features large features completed" >> "$archive_report"
        echo "- **Last Month**: $last_month_features large features completed" >> "$archive_report"
        
        if [[ $last_month_features -gt 0 ]]; then
            if [[ $this_month_features -gt $last_month_features ]]; then
                echo "- **Trend**: ↗️ Increasing large feature velocity" >> "$archive_report"
            elif [[ $this_month_features -lt $last_month_features ]]; then
                echo "- **Trend**: ↘️ Decreasing large feature velocity" >> "$archive_report"
            else
                echo "- **Trend**: ➡️ Steady large feature velocity" >> "$archive_report"
            fi
        fi
    fi
    
    echo "" >> "$archive_report"
    echo "### Large Feature Archive Recommendations" >> "$archive_report"
    
    if [[ -z "$found_archives" ]]; then
        echo "- 🏗️ **Create feature archive structure** for completed large features" >> "$archive_report"
        echo "- 📝 **Establish comprehensive archival process** including context packages and worktree bundles" >> "$archive_report"
    else
        echo "- 🧹 **Archive Maintenance**: Review feature archives older than 1 year for compression" >> "$archive_report"
        echo "- 📊 **Pattern Library**: Extract successful context patterns for reuse" >> "$archive_report"
        echo "- 🎯 **Methodology Refinement**: Use archive data to improve subtask decomposition" >> "$archive_report"
        echo "- 📚 **Knowledge Base**: Create searchable index of successful large feature approaches" >> "$archive_report"
    fi
    
    echo "**Large feature archive report saved to**: $archive_report"
    echo "**View with**: cat $archive_report"
}

# Execute large feature archive analysis
generate_large_feature_archive_summary
```

## Integration Status Monitoring

### Cross-Task Integration Health
```bash
# Monitor integration health across parallel subtasks
monitor_integration_health() {
    echo "=== INTEGRATION HEALTH MONITORING ==="
    
    # Check for integration test results
    echo "🔗 Integration Test Status:"
    for feature_dir in ~/work/worktrees/*/; do
        if [[ -d "$feature_dir" ]]; then
            local feature_name=$(basename "$feature_dir")
            echo "Feature: $feature_name"
            
            # Look for integration test results
            if find "$feature_dir" -name "*integration*test*" -o -name "*test*integration*" 2>/dev/null | grep -q .; then
                echo "  ✅ Integration tests found"
                
                # Check recent test results
                if find "$feature_dir" -name "*.log" -o -name "*results*" -newermt "$(date -d '1 hour ago' '+%Y-%m-%d %H:%M')" 2>/dev/null | grep -q .; then
                    echo "  🔄 Recent test activity detected"
                fi
            else
                echo "  ⚠️ No integration tests found"
            fi
            
            # Check for API contract validation
            if find "$feature_dir" -name "*schema*" -o -name "*contract*" -o -name "*api*" 2>/dev/null | grep -q .; then
                echo "  📋 API contracts present"
            fi
        fi
    done
    
    # Check for cross-feature dependencies
    echo
    echo "🔄 Cross-Feature Dependencies:"
    find . -name "*.md" -exec grep -H "depends.*feature\|requires.*feature\|blocks.*feature" {} \; 2>/dev/null | while read dependency; do
        echo "  ⚠️ Dependency found: $dependency"
    done
}

monitor_integration_health
```

## Status Summary Dashboard (Large Features)

### Quick Large Feature Status Commands
```bash
# Specialized commands for large feature monitoring

# Large feature count
alias large-feature-count='find . -name "context" -type d | wc -l | xargs echo "Active large features:"'

# Context completeness check
alias context-health='find . -name "context" -type d | while read ctx; do echo "$(dirname "$ctx" | xargs basename):"; for f in codebase-analysis.md external-sources.md implementation-patterns.md architecture-decisions.md; do if [[ -f "$ctx/$f" ]]; then echo "  ✅ $f"; else echo "  ❌ $f"; fi; done; done'

# Subagent coordination status
alias subagent-status='for d in ~/work/worktrees/*/; do if [[ -d "$d" ]]; then echo "$(basename "$d"):"; find "$d" -mindepth 1 -maxdepth 1 -type d | while read t; do echo "  $(basename "$t"): $(if [[ -f "$t/completion_report.md" ]]; then echo "Complete"; elif [[ -f "$t/status.txt" ]]; then cat "$t/status.txt"; else echo "Unknown"; fi)"; done; fi; done'

# Integration readiness
alias integration-ready='for d in ~/work/worktrees/*/; do if [[ -d "$d" ]]; then total=$(find "$d" -mindepth 1 -maxdepth 1 -type d | wc -l); complete=$(find "$d" -mindepth 1 -maxdepth 1 -type d -name "*" -exec test -f {}/completion_report.md \; -print | wc -l); echo "$(basename "$d"): $complete/$total ready"; fi; done'

# Archive feature count
alias feature-archive-count='for dir in /archive/features ./archive/features ~/archive/features; do if [[ -d "$dir" ]]; then count=$(find "$dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l); echo "Feature archive $dir: $count features"; fi; done'

echo "Large feature status commands available:"
echo "  large-feature-count    - Count active large features"
echo "  context-health         - Check context package completeness"
echo "  subagent-status        - Show subagent coordination status"
echo "  integration-ready      - Show integration readiness"
echo "  feature-archive-count  - Count archived large features"
```

## Usage Instructions (Large Features)

### Generate Complete Large Feature Status Report
```bash
# Run comprehensive large feature analysis
echo "Generating complete large feature status report..."
bash -c "$(grep -A 100 'generate_large_feature_status_report()' status.md | grep -A 100 '{')"
echo
echo "Generating large feature archive summary..."
bash -c "$(grep -A 200 'generate_large_feature_archive_summary()' status.md | grep -A 200 '{')"
echo
echo "Analyzing subagent coordination..."
bash -c "$(grep -A 50 'analyze_subagent_coordination()' status.md | grep -A 50 '{')"
```

### Monitor Large Feature Development
```bash
# Setup continuous large feature monitoring
monitor_large_feature_development() {
    while true; do
        clear
        echo "=== LARGE FEATURE DEVELOPMENT MONITOR ==="
        echo "Last updated: $(date)"
        echo
        
        # Quick feature count
        local active_features=$(find . -name "context" -type d 2>/dev/null | wc -l)
        local worktree_features=$(find ~/work/worktrees -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
        
        echo "🏗️ Active large features: $active_features"
        echo "🌳 Worktree features: $worktree_features"
        
        # Integration status
        echo "🔗 Integration status:"
        for d in ~/work/worktrees/*/; do
            if [[ -d "$d" ]]; then
                local feature_name=$(basename "$d")
                local total=$(find "$d" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
                local complete=$(find "$d" -name "completion_report.md" 2>/dev/null | wc -l)
                echo "  $feature_name: $complete/$total subtasks complete"
            fi
        done
        
        # Recent activity
        echo "📝 Recent context updates:"
        find . -name "context" -type d -exec find {} -name "*.md" -newermt "$(date -d '2 hours ago' '+%Y-%m-%d %H:%M')" \; 2>/dev/null | head -3
        
        sleep 600  # Update every 10 minutes for large features
    done
}

# Uncomment to run: monitor_large_feature_development &
```

This large feature status system provides comprehensive tracking and analysis specifically designed for complex features using the subtask methodology, enabling effective coordination of parallel development and integration monitoring.