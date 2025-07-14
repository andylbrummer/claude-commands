# Go MCP StdIO Server Setup

Initialize a simple Model Context Protocol (MCP) server in Go using stdio transport.

## Usage

```bash
cd $ARGUMENTS  # Navigate to target directory
```

## Dependencies

Choose one of these popular Go MCP libraries:

### Option 1: mark3labs/mcp-go (Recommended)
```bash
go mod init my-mcp-server
go get github.com/mark3labs/mcp-go
```

### Option 2: metoro-io/mcp-golang
```bash
go mod init my-mcp-server
go get github.com/metoro-io/mcp-golang
```

## Basic Server Implementation

### Using mark3labs/mcp-go

```go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/mark3labs/mcp-go/mcp"
    "github.com/mark3labs/mcp-go/server"
)

func main() {
    // Handle CLI commands and configuration
    if len(os.Args) > 1 {
        switch os.Args[1] {
        case "-h", "--help":
            printUsage()
            return
        case "config":
            handleConfigCommand()
            return
        case "env":
            handleEnvCommand()
            return
        case "test":
            handleTestCommand()
            return
        case "tools":
            handleToolsCommand()
            return
        default:
            if strings.HasPrefix(os.Args[1], "-") {
                fmt.Printf("Unknown flag: %s\n", os.Args[1])
                printUsage()
                return
            }
        }
    }

    // Detect if running from CLI vs MCP client
    if isRunningFromCLI() {
        fmt.Printf("MCP Server: My MCP Server v1.0.0\n")
        fmt.Printf("This is an MCP (Model Context Protocol) server.\n")
        fmt.Printf("It should be run by an MCP client, not directly from the command line.\n\n")
        printUsage()
        return
    }

    // Create MCP server
    s := server.NewMCPServer(
        "My MCP Server",
        "1.0.0",
        server.WithToolCapabilities(false),
    )

    // Define tool
    tool := mcp.NewTool("hello_world",
        mcp.WithDescription("Say hello to someone"),
        mcp.WithString("name",
            mcp.Required(),
            mcp.Description("Name of person to greet"),
        ),
    )

    // Add tool handler
    s.AddTool(tool, helloHandler)

    // Start stdio server
    if err := server.ServeStdio(s); err != nil {
        fmt.Printf("Server error: %v\n", err)
    }
}

func helloHandler(ctx context.Context, request mcp.CallToolRequest) (*mcp.CallToolResult, error) {
    name, err := request.RequireString("name")
    if err != nil {
        return mcp.NewToolResultError(err.Error()), nil
    }

    return mcp.NewToolResultText(fmt.Sprintf("Hello, %s!", name)), nil
}

// isRunningFromCLI detects if the program is running from command line vs MCP client
func isRunningFromCLI() bool {
    // Check if stdin is a terminal (tty)
    if fileInfo, err := os.Stdin.Stat(); err == nil {
        return (fileInfo.Mode() & os.ModeCharDevice) != 0
    }
    return true
}

// printUsage displays help information for CLI usage
func printUsage() {
    fmt.Printf(`USAGE:
    This MCP server is designed to work with MCP clients like Claude Desktop.
    
    For direct testing:
    %s --help           Show this help message
    
    For MCP client usage:
    1. Build the server:
       go build -o mcp-server
    
    2. Add to your MCP client configuration:
       Claude Desktop: Add to claude_desktop_config.json
       {
         "mcpServers": {
           "my-mcp-server": {
             "command": "/path/to/mcp-server"
           }
         }
       }
    
    3. Start your MCP client (Claude Desktop, etc.)
    
    Available Tools:
    - hello_world: Say hello to someone
    
    Environment Variables:
    - MCP_DEBUG=1: Enable debug logging
    
    For more information about MCP:
    https://modelcontextprotocol.io/
`, os.Args[0])
}
```

### Using metoro-io/mcp-golang

```go
package main

import (
    "fmt"
    "os"
    "strings"
    
    "github.com/metoro-io/mcp-golang"
    "github.com/metoro-io/mcp-golang/transport/stdio"
)

type HelloArgs struct {
    Name string `json:"name" jsonschema:"required,description=Name of person to greet"`
}

func main() {
    // Handle CLI commands and configuration
    if len(os.Args) > 1 {
        switch os.Args[1] {
        case "-h", "--help":
            printUsage()
            return
        case "config":
            handleConfigCommand()
            return
        case "env":
            handleEnvCommand()
            return
        case "test":
            handleTestCommand()
            return
        case "tools":
            handleToolsCommand()
            return
        default:
            if strings.HasPrefix(os.Args[1], "-") {
                fmt.Printf("Unknown flag: %s\n", os.Args[1])
                printUsage()
                return
            }
        }
    }

    // Detect if running from CLI vs MCP client
    if isRunningFromCLI() {
        fmt.Printf("MCP Server: My MCP Server v1.0.0\n")
        fmt.Printf("This is an MCP (Model Context Protocol) server.\n")
        fmt.Printf("It should be run by an MCP client, not directly from the command line.\n\n")
        printUsage()
        return
    }

    // Create server with stdio transport
    server := mcp_golang.NewServer(stdio.NewStdioServerTransport())

    // Register tool
    err := server.RegisterTool("hello_world", "Say hello to someone", func(args HelloArgs) (*mcp_golang.ToolResponse, error) {
        return mcp_golang.NewToolResponse(
            mcp_golang.NewTextContent(
                fmt.Sprintf("Hello, %s!", args.Name),
            ),
        ), nil
    })

    if err != nil {
        panic(err)
    }

    // Start server
    if err := server.Serve(); err != nil {
        panic(err)
    }
}

// isRunningFromCLI detects if the program is running from command line vs MCP client
func isRunningFromCLI() bool {
    // Check if stdin is a terminal (tty)
    if fileInfo, err := os.Stdin.Stat(); err == nil {
        return (fileInfo.Mode() & os.ModeCharDevice) != 0
    }
    return true
}

// printUsage displays help information for CLI usage
func printUsage() {
    fmt.Printf(`USAGE:
    This MCP server is designed to work with MCP clients like Claude Desktop.
    
    For direct testing:
    %s --help           Show this help message
    
    For MCP client usage:
    1. Build the server:
       go build -o mcp-server
    
    2. Add to your MCP client configuration:
       Claude Desktop: Add to claude_desktop_config.json
       {
         "mcpServers": {
           "my-mcp-server": {
             "command": "/path/to/mcp-server"
           }
         }
       }
    
    3. Start your MCP client (Claude Desktop, etc.)
    
    Available Tools:
    - hello_world: Say hello to someone
    
    Environment Variables:
    - MCP_DEBUG=1: Enable debug logging
    
    For more information about MCP:
    https://modelcontextprotocol.io/
`, os.Args[0])
}
```

## Build and Run

```bash
go build -o mcp-server
./mcp-server
```

## CLI Detection and Help

The server includes CLI detection to provide helpful information when run directly:

### Running from Command Line
```bash
# Show help information
./mcp-server --help

# Running without arguments shows info and usage
./mcp-server
```

### Expected Output
```
MCP Server: My MCP Server v1.0.0
This is an MCP (Model Context Protocol) server.
It should be run by an MCP client, not directly from the command line.

USAGE:
    This MCP server is designed to work with MCP clients like Claude Desktop.
    
    For direct testing:
    ./mcp-server --help           Show this help message
    
    For MCP client usage:
    1. Build the server:
       go build -o mcp-server
    
    2. Add to your MCP client configuration:
       Claude Desktop: Add to claude_desktop_config.json
       {
         "mcpServers": {
           "my-mcp-server": {
             "command": "/path/to/mcp-server"
           }
         }
       }
    
    3. Start your MCP client (Claude Desktop, etc.)
    
    Available Tools:
    - hello_world: Say hello to someone
    
    Environment Variables:
    - MCP_DEBUG=1: Enable debug logging
    
    For more information about MCP:
    https://modelcontextprotocol.io/
```

### Detection Logic
The server detects CLI usage by checking if stdin is a terminal (TTY). When run by an MCP client, stdin is typically redirected and not a terminal.

## Testing MCP Servers

### Manual Testing with MCP Inspector
```bash
# Install MCP Inspector for testing
npm install -g @modelcontextprotocol/inspector

# Test your server
mcp-inspector /path/to/mcp-server
```

### Testing with Claude Desktop
1. Add server to configuration
2. Restart Claude Desktop
3. Use the tools in conversations

### Environment Variables for Testing
```bash
# Enable debug logging
MCP_DEBUG=1 ./mcp-server

# Test with custom configuration
MCP_CONFIG_PATH=/path/to/config.json ./mcp-server
```

## Best Practices

1. **Type Safety**: Use strongly typed structs for tool arguments
2. **Error Handling**: Always handle errors gracefully
3. **Tool Names**: Use descriptive names following `^[a-zA-Z0-9_-]{1,128}$` pattern
4. **Recovery**: Add `server.WithRecovery()` for production servers
5. **Validation**: Validate input parameters before processing
6. **Documentation**: Use clear descriptions for tools and parameters

## Transport Notes

- **stdio**: Uses standard input/output streams (default, simplest)
- **SSE**: Server-sent events for web-based clients
- **HTTP**: Stateless communication for web services

## Configuration for Claude Desktop

Add to Claude Desktop config:
```json
{
    "mcpServers": {
        "my-mcp-server": {
            "command": "/path/to/mcp-server"
        }
    }
}
```

## Advanced CLI Features

### Configuration Management
```go
// handleConfigCommand manages configuration files
func handleConfigCommand() {
    if len(os.Args) < 3 {
        fmt.Printf(`Configuration Management:
    %s config init              Create default configuration file
    %s config show              Show current configuration
    %s config set <key> <value> Set configuration value
    %s config get <key>         Get configuration value
    %s config validate          Validate configuration file
    %s config path              Show configuration file path
    
Example:
    %s config init
    %s config set api_key "your-api-key"
    %s config set database_url "postgres://localhost/mydb"
`, os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0])
        return
    }
    
    switch os.Args[2] {
    case "init":
        fmt.Println("Creating default configuration file...")
        // Implementation: Create config.json with default values
        fmt.Println("Configuration file created at: ./config.json")
    case "show":
        fmt.Println("Current configuration:")
        // Implementation: Display current config
    case "set":
        if len(os.Args) < 5 {
            fmt.Println("Usage: config set <key> <value>")
            return
        }
        key, value := os.Args[3], os.Args[4]
        fmt.Printf("Setting %s = %s\n", key, value)
        // Implementation: Set config value
    case "get":
        if len(os.Args) < 4 {
            fmt.Println("Usage: config get <key>")
            return
        }
        key := os.Args[3]
        fmt.Printf("Getting value for %s\n", key)
        // Implementation: Get config value
    case "validate":
        fmt.Println("Validating configuration...")
        // Implementation: Validate config file
    case "path":
        fmt.Println("Configuration file path: ./config.json")
    default:
        fmt.Printf("Unknown config command: %s\n", os.Args[2])
    }
}

// handleEnvCommand manages environment variables
func handleEnvCommand() {
    if len(os.Args) < 3 {
        fmt.Printf(`Environment Variable Management:
    %s env list           List all environment variables
    %s env check          Check required environment variables
    %s env template       Generate .env template file
    %s env validate       Validate environment variables
    
Example:
    %s env check
    %s env template > .env
`, os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0])
        return
    }
    
    switch os.Args[2] {
    case "list":
        fmt.Println("Environment variables:")
        fmt.Printf("MCP_DEBUG: %s\n", os.Getenv("MCP_DEBUG"))
        fmt.Printf("MCP_CONFIG_PATH: %s\n", os.Getenv("MCP_CONFIG_PATH"))
        // Add other relevant env vars
    case "check":
        fmt.Println("Checking required environment variables...")
        // Implementation: Check if required env vars are set
        fmt.Println("✓ All required environment variables are set")
    case "template":
        fmt.Println(`# MCP Server Environment Variables
# Copy this file to .env and fill in your values

# Debug logging (0 or 1)
MCP_DEBUG=0

# Configuration file path
MCP_CONFIG_PATH=./config.json

# API Keys (if needed)
# API_KEY=your-api-key-here
# DATABASE_URL=your-database-url-here`)
    case "validate":
        fmt.Println("Validating environment variables...")
        // Implementation: Validate env vars
    default:
        fmt.Printf("Unknown env command: %s\n", os.Args[2])
    }
}

// handleTestCommand provides CLI testing of MCP tools
func handleTestCommand() {
    if len(os.Args) < 3 {
        fmt.Printf(`Tool Testing:
    %s test list                List available tools
    %s test <tool> [args...]    Test specific tool
    
Example:
    %s test hello_world name="John"
`, os.Args[0], os.Args[0], os.Args[0])
        return
    }
    
    switch os.Args[2] {
    case "list":
        fmt.Println("Available tools:")
        fmt.Println("- hello_world: Say hello to someone")
        // Implementation: List all available tools
    default:
        toolName := os.Args[2]
        fmt.Printf("Testing tool: %s\n", toolName)
        
        if toolName == "hello_world" {
            name := "World"
            if len(os.Args) > 3 {
                // Simple argument parsing for demo
                for _, arg := range os.Args[3:] {
                    if strings.HasPrefix(arg, "name=") {
                        name = strings.TrimPrefix(arg, "name=")
                        name = strings.Trim(name, "\"'")
                    }
                }
            }
            fmt.Printf("Result: Hello, %s!\n", name)
        } else {
            fmt.Printf("Unknown tool: %s\n", toolName)
        }
    }
}

// handleToolsCommand provides CLI interface to MCP tools
func handleToolsCommand() {
    if len(os.Args) < 3 {
        fmt.Printf(`Tool Interface:
    %s tools list               List all available tools
    %s tools describe <tool>    Show tool description and parameters
    %s tools run <tool> [args]  Run tool with arguments
    
Example:
    %s tools list
    %s tools describe hello_world
    %s tools run hello_world --name "John"
`, os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0], os.Args[0])
        return
    }
    
    switch os.Args[2] {
    case "list":
        fmt.Println("Available MCP Tools:")
        fmt.Println()
        fmt.Println("hello_world")
        fmt.Println("  Description: Say hello to someone")
        fmt.Println("  Parameters:")
        fmt.Println("    - name (string, required): Name of person to greet")
        fmt.Println()
        // Implementation: List all tools with descriptions
    case "describe":
        if len(os.Args) < 4 {
            fmt.Println("Usage: tools describe <tool>")
            return
        }
        toolName := os.Args[3]
        if toolName == "hello_world" {
            fmt.Println("Tool: hello_world")
            fmt.Println("Description: Say hello to someone")
            fmt.Println("Parameters:")
            fmt.Println("  - name (string, required): Name of person to greet")
            fmt.Println()
            fmt.Println("Example usage:")
            fmt.Printf("  %s tools run hello_world --name \"John\"\n", os.Args[0])
        } else {
            fmt.Printf("Unknown tool: %s\n", toolName)
        }
    case "run":
        if len(os.Args) < 4 {
            fmt.Println("Usage: tools run <tool> [args]")
            return
        }
        toolName := os.Args[3]
        fmt.Printf("Running tool: %s\n", toolName)
        
        if toolName == "hello_world" {
            name := "World"
            // Parse CLI arguments (simple implementation)
            for i := 4; i < len(os.Args); i++ {
                if os.Args[i] == "--name" && i+1 < len(os.Args) {
                    name = os.Args[i+1]
                    i++ // Skip next arg as it's the value
                }
            }
            fmt.Printf("Result: Hello, %s!\n", name)
        } else {
            fmt.Printf("Unknown tool: %s\n", toolName)
        }
    default:
        fmt.Printf("Unknown tools command: %s\n", os.Args[2])
    }
}
```

## CLI Usage Examples

Once you've implemented the CLI features, your MCP server becomes a powerful dual-purpose tool:

### Configuration Management
```bash
# Initialize configuration
./mcp-server config init

# Set API keys and database URLs
./mcp-server config set api_key "sk-1234567890abcdef"
./mcp-server config set database_url "postgres://user:pass@localhost/mydb"

# View current configuration
./mcp-server config show

# Validate configuration
./mcp-server config validate
```

### Environment Variables
```bash
# Generate .env template
./mcp-server env template > .env

# Check required environment variables
./mcp-server env check

# List all environment variables
./mcp-server env list
```

### Testing Tools
```bash
# List available tools
./mcp-server tools list

# Get detailed information about a tool
./mcp-server tools describe hello_world

# Run tools directly from CLI
./mcp-server tools run hello_world --name "Alice"
./mcp-server test hello_world name="Bob"
```

### Development Benefits

1. **Faster Development**: Test tools without MCP client setup
2. **Configuration Management**: Easy setup and validation
3. **Debugging**: Direct access to tool functionality
4. **CI/CD Integration**: Automated testing and validation
5. **Documentation**: Built-in help and examples

### Production Considerations

```go
// Add version information
const Version = "1.0.0"

// Add build information
var (
    BuildTime = "unknown"
    GitCommit = "unknown"
)

// Enhanced version command
func handleVersionCommand() {
    fmt.Printf("MCP Server v%s\n", Version)
    fmt.Printf("Build time: %s\n", BuildTime)
    fmt.Printf("Git commit: %s\n", GitCommit)
}
```

### Security Notes

- Store sensitive configuration in environment variables
- Use secure file permissions for config files (600)
- Validate all CLI inputs to prevent injection attacks
- Consider using libraries like `cobra` for complex CLI interfaces
- Implement proper logging for audit trails

This dual-mode approach makes your MCP server both a powerful development tool and a production-ready service.