# Task Status Reporting & Archive Summary

## Quick Status Overview

### Current Tasks Status Check
```bash
# Generate immediate status overview
echo "=== CURRENT TASKS STATUS OVERVIEW ==="
echo "Generated on: $(date)"
echo

# Check for active todo files
echo "Active Todo Files:"
find . -name "todo.md" -o -name "*todo*.md" 2>/dev/null | while read file; do
    if [[ -f "$file" ]]; then
        echo "📋 Found: $file"
        # Extract status if present
        if grep -q "Status:" "$file" 2>/dev/null; then
            status=$(grep "Status:" "$file" | head -1 | cut -d: -f2 | xargs)
            echo "   Status: $status"
        fi
        # Count incomplete tasks
        incomplete=$(grep -c "^- \[ \]" "$file" 2>/dev/null || echo "0")
        complete=$(grep -c "^- \[x\]" "$file" 2>/dev/null || echo "0")
        echo "   Tasks: $complete completed, $incomplete remaining"
    fi
done

# Check for any work in progress
echo
echo "Work in Progress Indicators:"
find . -name "*.md" -exec grep -l "in progress\|IN PROGRESS\|🔄" {} \; 2>/dev/null | while read file; do
    echo "🔄 $file contains work in progress markers"
done

echo
echo "=== END STATUS OVERVIEW ==="
```

## Comprehensive Todo Status Report

### MANDATORY: Todo Document Analysis
```markdown
## Todo Status Analysis Template

### Active Todo Documents Discovery
- **Search Locations**: Current directory, subdirectories, common todo locations
- **File Patterns**: todo.md, *todo*.md, task-*.md, *-tasks.md
- **Status Indicators**: Status fields, completion markers, progress indicators

### Todo Content Analysis
```bash
# Comprehensive todo analysis script
generate_todo_status_report() {
    local report_file="task-status-report-$(date +%Y%m%d-%H%M).md"
    
    cat > "$report_file" << 'EOF'
# Task Status Report
**Generated**: $(date)
**Location**: $(pwd)

## Executive Summary
EOF

    echo "## Executive Summary" >> "$report_file"
    
    # Count total todo files
    local todo_count=$(find . -name "*.md" -exec grep -l "^- \[ \]\|^- \[x\]" {} \; 2>/dev/null | wc -l)
    echo "- **Total Todo Files**: $todo_count" >> "$report_file"
    
    # Count total tasks
    local total_tasks=$(find . -name "*.md" -exec grep -c "^- \[ \]\|^- \[x\]" {} \; 2>/dev/null | awk '{sum+=$1} END {print sum+0}')
    local completed_tasks=$(find . -name "*.md" -exec grep -c "^- \[x\]" {} \; 2>/dev/null | awk '{sum+=$1} END {print sum+0}')
    local remaining_tasks=$((total_tasks - completed_tasks))
    
    echo "- **Total Tasks**: $total_tasks" >> "$report_file"
    echo "- **Completed Tasks**: $completed_tasks" >> "$report_file"
    echo "- **Remaining Tasks**: $remaining_tasks" >> "$report_file"
    
    if [[ $total_tasks -gt 0 ]]; then
        local completion_percentage=$((completed_tasks * 100 / total_tasks))
        echo "- **Completion Rate**: ${completion_percentage}%" >> "$report_file"
    fi
    
    echo "" >> "$report_file"
    echo "## Detailed Todo Analysis" >> "$report_file"
    
    # Analyze each todo file
    find . -name "*.md" -exec grep -l "^- \[ \]\|^- \[x\]" {} \; 2>/dev/null | while read todo_file; do
        echo "### File: $todo_file" >> "$report_file"
        
        # Extract metadata
        if grep -q "^# \|^## " "$todo_file"; then
            local title=$(grep "^# \|^## " "$todo_file" | head -1 | sed 's/^#* //')
            echo "**Title**: $title" >> "$report_file"
        fi
        
        # Check for status indicators
        if grep -q "Status:\|**Status**:" "$todo_file"; then
            local status=$(grep "Status:\|**Status**:" "$todo_file" | head -1 | sed 's/.*Status[*:]*[[:space:]]*//' | sed 's/\*\*$//')
            echo "**Status**: $status" >> "$report_file"
        fi
        
        # Check for priority indicators
        if grep -q "Priority:\|**Priority**:" "$todo_file"; then
            local priority=$(grep "Priority:\|**Priority**:" "$todo_file" | head -1 | sed 's/.*Priority[*:]*[[:space:]]*//' | sed 's/\*\*$//')
            echo "**Priority**: $priority" >> "$report_file"
        fi
        
        # Count tasks in this file
        local file_total=$(grep -c "^- \[ \]\|^- \[x\]" "$todo_file" 2>/dev/null)
        local file_completed=$(grep -c "^- \[x\]" "$todo_file" 2>/dev/null)
        local file_remaining=$((file_total - file_completed))
        
        echo "**Tasks**: $file_completed/$file_total completed" >> "$report_file"
        
        # List incomplete tasks
        if [[ $file_remaining -gt 0 ]]; then
            echo "**Remaining Tasks**:" >> "$report_file"
            grep "^- \[ \]" "$todo_file" | head -5 | sed 's/^/  /' >> "$report_file"
            if [[ $file_remaining -gt 5 ]]; then
                echo "  ... and $((file_remaining - 5)) more" >> "$report_file"
            fi
        fi
        
        # Check for blockers or issues
        if grep -qi "blocked\|blocker\|issue\|problem\|error" "$todo_file"; then
            echo "**⚠️ Potential Issues Found**:" >> "$report_file"
            grep -i "blocked\|blocker\|issue\|problem\|error" "$todo_file" | head -3 | sed 's/^/  /' >> "$report_file"
        fi
        
        echo "" >> "$report_file"
    done
    
    # Add recommendations
    echo "## Recommendations" >> "$report_file"
    if [[ $remaining_tasks -eq 0 ]]; then
        echo "✅ **All tasks completed** - Ready to proceed to review phase" >> "$report_file"
    elif [[ $remaining_tasks -le 3 ]]; then
        echo "🔄 **Nearly complete** - Focus on remaining $remaining_tasks tasks" >> "$report_file"
    else
        echo "📋 **Active development** - $remaining_tasks tasks remaining" >> "$report_file"
        echo "- Prioritize high-impact tasks" >> "$report_file"
        echo "- Check for any blocked tasks requiring attention" >> "$report_file"
        echo "- Consider breaking down large tasks if needed" >> "$report_file"
    fi
    
    echo "**Report saved to**: $report_file"
    echo "**View with**: cat $report_file"
}

# Execute the function
generate_todo_status_report
```

### Priority and Risk Assessment
```bash
# Analyze task priorities and risks
analyze_task_priorities() {
    echo "=== TASK PRIORITY & RISK ANALYSIS ==="
    
    # High priority tasks
    echo "🔥 HIGH PRIORITY TASKS:"
    find . -name "*.md" -exec grep -H -i "priority.*high\|high.*priority\|urgent\|critical" {} \; | head -10
    
    echo
    echo "⚠️ RISK INDICATORS:"
    find . -name "*.md" -exec grep -H -i "risk\|blocker\|blocked\|issue\|problem" {} \; | head -10
    
    echo
    echo "🕐 TIME-SENSITIVE TASKS:"
    find . -name "*.md" -exec grep -H -i "deadline\|due\|urgent\|asap" {} \; | head -10
}

analyze_task_priorities
```

## Archive Summary Report

### MANDATORY: Archive Analysis
```bash
# Generate comprehensive archive summary
generate_archive_summary() {
    local archive_report="archive-summary-$(date +%Y%m%d-%H%M).md"
    
    cat > "$archive_report" << 'EOF'
# Archive Summary Report
**Generated**: $(date)
**Archive Locations Scanned**: /archive, ./archive, ../archive, ~/archive

## Archive Overview
EOF

    echo "## Archive Overview" >> "$archive_report"
    
    # Find archive directories
    local archive_dirs=""
    for dir in "/archive" "./archive" "../archive" "~/archive" "/home/beagle/.claude/archive"; do
        if [[ -d "$dir" ]]; then
            archive_dirs="$archive_dirs $dir"
        fi
    done
    
    if [[ -z "$archive_dirs" ]]; then
        echo "❌ **No archive directories found**" >> "$archive_report"
        echo "- Searched locations: /archive, ./archive, ../archive, ~/archive" >> "$archive_report"
        echo "- Consider creating archive structure for completed work" >> "$archive_report"
    else
        echo "📁 **Archive Directories Found**: $archive_dirs" >> "$archive_report"
        
        # Analyze each archive directory
        for archive_dir in $archive_dirs; do
            echo "" >> "$archive_report"
            echo "### Archive: $archive_dir" >> "$archive_report"
            
            if [[ -d "$archive_dir" ]]; then
                local total_items=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l)
                echo "**Total Archived Items**: $total_items" >> "$archive_report"
                
                # Recent archives (last 30 days)
                local recent_count=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -newermt "$(date -d '30 days ago' '+%Y-%m-%d')" 2>/dev/null | wc -l)
                echo "**Recent Archives (30 days)**: $recent_count" >> "$archive_report"
                
                # List recent archived items
                echo "**Recent Archived Items**:" >> "$archive_report"
                find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -printf '%T@ %p\n' 2>/dev/null | \
                sort -rn | head -10 | while read timestamp path; do
                    local date_str=$(date -d "@$timestamp" '+%Y-%m-%d' 2>/dev/null || echo "unknown")
                    local item_name=$(basename "$path")
                    echo "- [$date_str] $item_name" >> "$archive_report"
                    
                    # Check for completion documentation
                    if [[ -f "$path/completion_report.md" ]] || [[ -f "$path/retrospective.md" ]]; then
                        echo "  ✅ Has completion documentation" >> "$archive_report"
                    fi
                done
                
                # Archive categories analysis
                echo "" >> "$archive_report"
                echo "**Archive Categories**:" >> "$archive_report"
                
                # Count by type (based on naming patterns)
                local task_archives=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -name "*task*" 2>/dev/null | wc -l)
                local feature_archives=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -name "*feature*" 2>/dev/null | wc -l)
                local bug_archives=$(find "$archive_dir" -mindepth 1 -maxdepth 1 -type d -name "*bug*" -o -name "*fix*" 2>/dev/null | wc -l)
                
                echo "- **Tasks**: $task_archives archived" >> "$archive_report"
                echo "- **Features**: $feature_archives archived" >> "$archive_report"
                echo "- **Bug Fixes**: $bug_archives archived" >> "$archive_report"
                
                # Check for archive quality
                echo "" >> "$archive_report"
                echo "**Archive Quality Assessment**:" >> "$archive_report"
                
                local documented_archives=0
                local total_archives=0
                
                find "$archive_dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | while read archived_item; do
                    total_archives=$((total_archives + 1))
                    
                    # Check for key documentation files
                    local has_completion=$(find "$archived_item" -name "*completion*" -o -name "*retrospective*" -o -name "*summary*" 2>/dev/null | wc -l)
                    local has_todo=$(find "$archived_item" -name "*todo*" 2>/dev/null | wc -l)
                    
                    if [[ $has_completion -gt 0 ]] && [[ $has_todo -gt 0 ]]; then
                        documented_archives=$((documented_archives + 1))
                    fi
                done
                
                if [[ $total_archives -gt 0 ]]; then
                    local quality_percentage=$((documented_archives * 100 / total_archives))
                    echo "- **Documentation Quality**: ${quality_percentage}% of archives have completion docs" >> "$archive_report"
                fi
            fi
        done
    fi
    
    # Add insights and recommendations
    echo "" >> "$archive_report"
    echo "## Insights & Recommendations" >> "$archive_report"
    
    if [[ -n "$archive_dirs" ]]; then
        echo "### Completed Work Insights" >> "$archive_report"
        
        # Extract common patterns from archive names
        echo "**Common Work Patterns**:" >> "$archive_report"
        find $archive_dirs -mindepth 1 -maxdepth 1 -type d 2>/dev/null | \
        basename -a | cut -d'-' -f3- | sort | uniq -c | sort -rn | head -5 | \
        while read count pattern; do
            echo "- $pattern ($count occurrences)" >> "$archive_report"
        done
        
        echo "" >> "$archive_report"
        echo "### Work Velocity Analysis" >> "$archive_report"
        
        # Monthly completion rate
        local current_month=$(date '+%Y-%m')
        local this_month_count=$(find $archive_dirs -mindepth 1 -maxdepth 1 -type d -name "${current_month}*" 2>/dev/null | wc -l)
        echo "- **This Month**: $this_month_count items completed" >> "$archive_report"
        
        local last_month=$(date -d 'last month' '+%Y-%m')
        local last_month_count=$(find $archive_dirs -mindepth 1 -maxdepth 1 -type d -name "${last_month}*" 2>/dev/null | wc -l)
        echo "- **Last Month**: $last_month_count items completed" >> "$archive_report"
        
        if [[ $last_month_count -gt 0 ]]; then
            if [[ $this_month_count -gt $last_month_count ]]; then
                echo "- **Trend**: ↗️ Increasing productivity" >> "$archive_report"
            elif [[ $this_month_count -lt $last_month_count ]]; then
                echo "- **Trend**: ↘️ Decreasing productivity" >> "$archive_report"
            else
                echo "- **Trend**: ➡️ Steady productivity" >> "$archive_report"
            fi
        fi
    fi
    
    echo "" >> "$archive_report"
    echo "### Archive Maintenance Recommendations" >> "$archive_report"
    
    if [[ -z "$archive_dirs" ]]; then
        echo "- 🏗️ **Create archive structure** to track completed work" >> "$archive_report"
        echo "- 📝 **Establish archival process** for finished tasks" >> "$archive_report"
    else
        echo "- 🧹 **Archive Cleanup**: Review archives older than 6 months for compression" >> "$archive_report"
        echo "- 📊 **Pattern Analysis**: Use archive data to improve future task planning" >> "$archive_report"
        echo "- 📚 **Knowledge Base**: Create searchable index of archived solutions" >> "$archive_report"
    fi
    
    echo "**Archive report saved to**: $archive_report"
    echo "**View with**: cat $archive_report"
}

# Execute archive analysis
generate_archive_summary
```

### Archive Quality Assessment
```bash
# Assess quality and completeness of archived work
assess_archive_quality() {
    echo "=== ARCHIVE QUALITY ASSESSMENT ==="
    
    local archive_dirs="/archive ./archive ../archive ~/archive /home/beagle/.claude/archive"
    
    for archive_dir in $archive_dirs; do
        if [[ -d "$archive_dir" ]]; then
            echo "📊 Analyzing archive quality in: $archive_dir"
            
            # Check each archived item
            find "$archive_dir" -mindepth 1 -maxdepth 1 -type d | while read archived_item; do
                local item_name=$(basename "$archived_item")
                echo
                echo "📁 $item_name"
                
                # Check for essential documentation
                local score=0
                local max_score=5
                
                if [[ -f "$archived_item/todo.md" ]] || [[ -f "$archived_item/task.md" ]]; then
                    echo "  ✅ Task documentation present"
                    score=$((score + 1))
                else
                    echo "  ❌ Missing task documentation"
                fi
                
                if find "$archived_item" -name "*completion*" -o -name "*complete*" | grep -q .; then
                    echo "  ✅ Completion documentation present"
                    score=$((score + 1))
                else
                    echo "  ❌ Missing completion documentation"
                fi
                
                if find "$archived_item" -name "*retrospective*" -o -name "*review*" | grep -q .; then
                    echo "  ✅ Retrospective documentation present"
                    score=$((score + 1))
                else
                    echo "  ❌ Missing retrospective documentation"
                fi
                
                if find "$archived_item" -name "*.md" -exec grep -l "lessons\|learning\|insight" {} \; | grep -q .; then
                    echo "  ✅ Learning capture present"
                    score=$((score + 1))
                else
                    echo "  ❌ Missing learning capture"
                fi
                
                if find "$archived_item" -type f | grep -q .; then
                    echo "  ✅ Contains files"
                    score=$((score + 1))
                else
                    echo "  ❌ Empty archive"
                fi
                
                local quality_percentage=$((score * 100 / max_score))
                echo "  📊 Quality Score: $score/$max_score ($quality_percentage%)"
                
                if [[ $quality_percentage -lt 60 ]]; then
                    echo "  ⚠️ Low quality archive - consider improving documentation"
                fi
            done
        fi
    done
}

assess_archive_quality
```

## Status Summary Dashboard

### Quick Status Commands
```bash
# One-liner status commands for quick reference

# Current todo status
alias todo-status='find . -name "*.md" -exec grep -c "^- \[ \]" {} \; | awk "{sum+=\$1} END {print \"Remaining tasks: \" sum+0}"'

# Completion rate
alias completion-rate='total=$(find . -name "*.md" -exec grep -c "^- \[ \]\|^- \[x\]" {} \; | awk "{sum+=\$1} END {print sum+0}"); completed=$(find . -name "*.md" -exec grep -c "^- \[x\]" {} \; | awk "{sum+=\$1} END {print sum+0}"); if [[ $total -gt 0 ]]; then echo "Completion: $completed/$total ($((completed*100/total))%)"; else echo "No tasks found"; fi'

# Archive count
alias archive-count='for dir in /archive ./archive ../archive ~/archive; do if [[ -d "$dir" ]]; then count=$(find "$dir" -mindepth 1 -maxdepth 1 -type d 2>/dev/null | wc -l); echo "Archive $dir: $count items"; fi; done'

# Recent activity
alias recent-work='find . -name "*.md" -newermt "$(date -d "7 days ago" "+%Y-%m-%d")" -exec ls -la {} \; | head -10'

echo "Quick status commands available:"
echo "  todo-status      - Count remaining tasks"
echo "  completion-rate  - Show completion percentage"
echo "  archive-count    - Count archived items"
echo "  recent-work      - Show recent file activity"
```

## Usage Instructions

### Generate Complete Status Report
```bash
# Run comprehensive status analysis
echo "Generating complete status report..."
bash -c "$(grep -A 50 'generate_todo_status_report()' status.md | grep -A 50 '{')"
echo
echo "Generating archive summary..."
bash -c "$(grep -A 100 'generate_archive_summary()' status.md | grep -A 100 '{')"
```

### Monitor Ongoing Work
```bash
# Setup continuous monitoring (run in background)
monitor_work_status() {
    while true; do
        clear
        echo "=== WORK STATUS MONITOR ==="
        echo "Last updated: $(date)"
        echo
        
        # Quick todo count
        local remaining=$(find . -name "*.md" -exec grep -c "^- \[ \]" {} \; 2>/dev/null | awk '{sum+=$1} END {print sum+0}')
        local completed=$(find . -name "*.md" -exec grep -c "^- \[x\]" {} \; 2>/dev/null | awk '{sum+=$1} END {print sum+0}')
        
        echo "📋 Tasks: $completed completed, $remaining remaining"
        
        # Recent file changes
        echo "📝 Recent changes:"
        find . -name "*.md" -newermt "$(date -d '1 hour ago' '+%Y-%m-%d %H:%M')" 2>/dev/null | head -5
        
        sleep 300  # Update every 5 minutes
    done
}

# Uncomment to run: monitor_work_status &
```

This status system provides comprehensive tracking of both active work and completed archives, enabling better project management and historical analysis.