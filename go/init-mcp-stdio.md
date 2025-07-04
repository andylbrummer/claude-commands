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
    
    "github.com/mark3labs/mcp-go/mcp"
    "github.com/mark3labs/mcp-go/server"
)

func main() {
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
```

### Using metoro-io/mcp-golang

```go
package main

import (
    "fmt"
    
    "github.com/metoro-io/mcp-golang"
    "github.com/metoro-io/mcp-golang/transport/stdio"
)

type HelloArgs struct {
    Name string `json:"name" jsonschema:"required,description=Name of person to greet"`
}

func main() {
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
```

## Build and Run

```bash
go build -o mcp-server
./mcp-server
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