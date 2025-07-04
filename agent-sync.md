---
description: Sync configurations between agentic coding systems
allowed-tools: [Bash, Read, Write, Glob, Grep, LS, Edit, MultiEdit]
---

# Agent Sync Command

Synchronizes configuration files between popular agentic coding systems. Use `$ARGUMENTS` to specify sync mode or get summaries.

## Usage
- `/user:agent-sync` - Default: summarize all configurations in current project
- `/user:agent-sync sync` - Sync configurations between systems
- `/user:agent-sync configure` - Interactive configuration setup
- `/user:agent-sync summary` - Detailed summary of all agent configs

## Supported Systems & Configuration Locations

### Claude Code
- **Global**: `~/.claude/CLAUDE.md`, `~/.claude/commands/`
- **Project**: `CLAUDE.md`, `.claude/settings.local.json`, `.claude/commands/`
- **Memory**: `.claude/memory.md`

### Cursor
- **Legacy**: `.cursorrules` (deprecated)
- **Modern**: `.cursor/rules/*.mdc` (recommended)
- **Format**: MDC (Markdown Components) with YAML frontmatter

### GitHub Copilot
- **Repository**: `.github/copilot-instructions.md`
- **VS Code**: `github-copilot.xml`, IDE settings
- **Chat**: Built-in VS Code settings for chat behavior

### Cline (Claude Dev)
- **Project**: `.clinerules` (project root)
- **VS Code**: Extension settings, custom instructions
- **Format**: Plain text project-specific guidelines

### Windsurf (Codeium)
- **Project**: `.windsurf/rules/` (similar to Cursor structure)
- **Format**: Rule files with MDC support

### Roo Code
- **Project**: `.roo/rules-{mode}/` (mode-specific rules)
- **Format**: Separate rule files per mode

### Aider
- **Project**: `.aider/` directory
- **Config**: `.aider.conf.yml`
- **Format**: YAML configuration

### Other Systems
- **OpenAI Codex**: API key configuration only
- **Augment Code**: `.augment/` directory
- **Gemini Code**: `.gemini/` directory

## Action Logic

!if [ -z "$ARGUMENTS" ]; then
    echo "=
 **Scanning for agent configurations...**"
    
    # Check for each system's config files
    find_configs() {
        echo "## Configuration Files Found:"
        echo ""
        
        # Claude Code
        [ -f ".claude/CLAUDE.md" ] && echo "-  Claude Code: \`.claude/CLAUDE.md\`"
        [ -d ".claude/commands" ] && echo "-  Claude Code Commands: \`.claude/commands/\`"
        
        # Cursor
        [ -f ".cursorrules" ] && echo "- � Cursor (Legacy): \`.cursorrules\` (deprecated)"
        [ -d ".cursor/rules" ] && echo "-  Cursor (Modern): \`.cursor/rules/\`"
        
        # GitHub Copilot
        [ -f ".github/copilot-instructions.md" ] && echo "-  GitHub Copilot: \`.github/copilot-instructions.md\`"
        
        # Cline
        [ -f ".clinerules" ] && echo "-  Cline: \`.clinerules\`"
        
        # Windsurf
        [ -d ".windsurf/rules" ] && echo "-  Windsurf: \`.windsurf/rules/\`"
        
        # Roo Code
        [ -d ".roo" ] && echo "-  Roo Code: \`.roo/\`"
        
        # Aider
        [ -f ".aider.conf.yml" ] && echo "-  Aider: \`.aider.conf.yml\`"
        [ -d ".aider" ] && echo "-  Aider: \`.aider/\`"
        
        # Other systems
        [ -d ".augment" ] && echo "-  Augment: \`.augment/\`"
        [ -d ".gemini" ] && echo "-  Gemini: \`.gemini/\`"
    }
    
    find_configs
    
    echo ""
    echo "## Next Steps:"
    echo "- Run \`/user:agent-sync sync\` to synchronize configurations"
    echo "- Run \`/user:agent-sync configure\` for interactive setup"
    echo "- Run \`/user:agent-sync summary\` for detailed analysis"
    
elif [ "$ARGUMENTS" = "sync" ]; then
    echo "= **Synchronizing agent configurations...**"
    
    # Create backup
    backup_dir=".agent-sync-backup-$(date +%Y%m%d-%H%M%S)"
    mkdir -p "$backup_dir"
    
    # Backup existing configs
    [ -f ".cursorrules" ] && cp ".cursorrules" "$backup_dir/"
    [ -f ".clinerules" ] && cp ".clinerules" "$backup_dir/"
    [ -f ".claude/CLAUDE.md" ] && cp ".claude/CLAUDE.md" "$backup_dir/"
    [ -f ".github/copilot-instructions.md" ] && cp ".github/copilot-instructions.md" "$backup_dir/"
    
    echo " Backup created in: $backup_dir"
    echo ""
    echo "**Configuration sync requires manual review and editing.**"
    echo "Please review and merge the following configurations:"
    echo ""
    
    # List all config files that need manual attention
    for config in .cursorrules .clinerules .claude/CLAUDE.md .github/copilot-instructions.md; do
        [ -f "$config" ] && echo "- $config"
    done
    
elif [ "$ARGUMENTS" = "configure" ]; then
    echo "� **Interactive Agent Configuration Setup**"
    echo ""
    echo "This will help you set up configuration files for multiple agentic coding systems."
    echo ""
    echo "**Available Systems:**"
    echo "1. Claude Code (.claude/CLAUDE.md)"
    echo "2. Cursor (.cursor/rules/)"
    echo "3. GitHub Copilot (.github/copilot-instructions.md)"
    echo "4. Cline (.clinerules)"
    echo "5. Windsurf (.windsurf/rules/)"
    echo "6. Roo Code (.roo/rules-{mode}/)"
    echo "7. Aider (.aider.conf.yml)"
    echo ""
    echo "**Setup Process:**"
    echo "1. Choose which systems to configure"
    echo "2. Define common coding standards"
    echo "3. Set project-specific requirements"
    echo "4. Generate configuration files"
    echo ""
    echo "Run this command with specific system names to configure individual systems."
    
elif [ "$ARGUMENTS" = "summary" ]; then
    echo "=� **Detailed Agent Configuration Summary**"
    echo ""
    
    # Function to analyze config content
    analyze_config() {
        local file="$1"
        local system="$2"
        
        if [ -f "$file" ]; then
            echo "### $system"
            echo "**File:** \`$file\`"
            echo "**Size:** $(wc -l < "$file") lines"
            echo "**Last Modified:** $(stat -c %y "$file" 2>/dev/null || stat -f %Sm "$file" 2>/dev/null || echo "Unknown")"
            echo ""
            
            # Show first few lines as preview
            echo "**Preview:**"
            echo "\`\`\`"
            head -5 "$file"
            echo "\`\`\`"
            echo ""
        else
            echo "### $system"
            echo "**Status:** Not configured"
            echo ""
        fi
    }
    
    # Analyze each system
    analyze_config ".claude/CLAUDE.md" "Claude Code"
    analyze_config ".cursorrules" "Cursor (Legacy)"
    analyze_config ".github/copilot-instructions.md" "GitHub Copilot"
    analyze_config ".clinerules" "Cline"
    analyze_config ".aider.conf.yml" "Aider"
    
    # Check for directory-based configs
    [ -d ".cursor/rules" ] && echo "### Cursor (Modern)\n**Directory:** \`.cursor/rules/\`\n**Files:** $(find .cursor/rules -name "*.mdc" 2>/dev/null | wc -l) rule files\n"
    [ -d ".windsurf/rules" ] && echo "### Windsurf\n**Directory:** \`.windsurf/rules/\`\n**Files:** $(find .windsurf/rules -type f 2>/dev/null | wc -l) rule files\n"
    [ -d ".roo" ] && echo "### Roo Code\n**Directory:** \`.roo/\`\n**Modes:** $(find .roo -name "rules-*" -type d 2>/dev/null | wc -l) configured modes\n"
    
    echo "## Configuration Differences"
    echo "**Key Differences Between Systems:**"
    echo "- **Claude Code**: Global + project-level, supports commands"
    echo "- **Cursor**: Moving to modular .mdc files in .cursor/rules/"
    echo "- **Copilot**: Repository-level instructions in .github/"
    echo "- **Cline**: Simple project-level .clinerules file"
    echo "- **Windsurf**: Similar to Cursor with rule files"
    echo "- **Roo Code**: Mode-specific rule organization"
    echo "- **Aider**: YAML configuration with tool-specific settings"
    
else
    echo "L **Unknown argument:** $ARGUMENTS"
    echo ""
    echo "**Valid arguments:**"
    echo "- \`sync\` - Synchronize configurations"
    echo "- \`configure\` - Interactive setup"
    echo "- \`summary\` - Detailed analysis"
    echo "- (no argument) - Quick scan"
fi

## Migration Guide

### From .cursorrules to .cursor/rules/
```bash
# Create new structure
mkdir -p .cursor/rules

# Split monolithic .cursorrules into focused rule files
# Example: general.mdc, typescript.mdc, testing.mdc
```

### From .clinerules to Claude Code
```bash
# Create Claude Code structure
mkdir -p .claude

# Convert .clinerules to .claude/CLAUDE.md
# Add project-specific instructions
```

### Cross-System Compatibility
- Use consistent naming conventions across all systems
- Maintain separate files for system-specific features
- Create a master instruction document for shared standards
- Use version control to track configuration changes

## Best Practices

1. **Version Control**: Always commit configuration files
2. **Documentation**: Document system-specific differences
3. **Backup**: Create backups before syncing
4. **Testing**: Test configurations with each system
5. **Maintenance**: Regular review and updates
6. **Team Sync**: Share configurations with team members

## Common Configuration Patterns

### Code Style Enforcement
```markdown
# Universal code style rules
- Use TypeScript for all new JavaScript code
- Follow ESLint and Prettier configurations
- Write unit tests for all functions
- Use descriptive variable names
```

### Project Architecture
```markdown
# Architecture guidelines
- Follow clean architecture principles
- Use dependency injection where appropriate
- Implement proper error handling
- Document API endpoints and interfaces
```

### Security Standards
```markdown
# Security requirements
- Never commit secrets or API keys
- Use environment variables for configuration
- Implement proper input validation
- Follow OWASP security guidelines
```

Remember: Each system has unique features, so not all configurations will translate perfectly. Review and adjust after syncing.