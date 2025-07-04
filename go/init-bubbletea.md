# Go Bubble Tea TUI Project Setup

Initialize a complete Bubble Tea TUI project with all essential components and styling.

## Usage

```bash
cd $ARGUMENTS  # Navigate to target directory
```

## Project Setup

```bash
go mod init my-tui-app
go get github.com/charmbracelet/bubbletea
go get github.com/charmbracelet/lipgloss
go get github.com/charmbracelet/bubbles/list
go get github.com/charmbracelet/bubbles/textinput
go get github.com/charmbracelet/bubbles/textarea
go get github.com/charmbracelet/bubbles/spinner
go get github.com/charmbracelet/bubbles/progress
go get github.com/charmbracelet/bubbles/table
go get github.com/charmbracelet/bubbles/viewport
go get github.com/charmbracelet/bubbles/help
go get github.com/charmbracelet/bubbles/key
```

## Complete Example Implementation

### main.go
```go
package main

import (
    "fmt"
    "os"
    "strings"

    tea "github.com/charmbracelet/bubbletea"
    "github.com/charmbracelet/lipgloss"
    "github.com/charmbracelet/bubbles/list"
    "github.com/charmbracelet/bubbles/textinput"
    "github.com/charmbracelet/bubbles/help"
    "github.com/charmbracelet/bubbles/key"
)

// Styles
var (
    titleStyle = lipgloss.NewStyle().
        Bold(true).
        Foreground(lipgloss.Color("#FAFAFA")).
        Background(lipgloss.Color("#7D56F4")).
        Padding(0, 1).
        MarginBottom(1)

    itemStyle = lipgloss.NewStyle().
        PaddingLeft(4)

    selectedItemStyle = lipgloss.NewStyle().
        PaddingLeft(2).
        Foreground(lipgloss.Color("#EE6FF8")).
        Bold(true)

    helpStyle = lipgloss.NewStyle().
        Foreground(lipgloss.Color("#626262"))

    statusStyle = lipgloss.NewStyle().
        Foreground(lipgloss.Color("#FFFDF5")).
        Background(lipgloss.Color("#FF5F87")).
        Padding(0, 1).
        MarginTop(1)
)

// Key bindings
type keyMap struct {
    Up    key.Binding
    Down  key.Binding
    Enter key.Binding
    Quit  key.Binding
    Help  key.Binding
}

var keys = keyMap{
    Up: key.NewBinding(
        key.WithKeys("k", "up"),
        key.WithHelp("�/k", "move up"),
    ),
    Down: key.NewBinding(
        key.WithKeys("j", "down"),
        key.WithHelp("�/j", "move down"),
    ),
    Enter: key.NewBinding(
        key.WithKeys("enter"),
        key.WithHelp("enter", "select"),
    ),
    Quit: key.NewBinding(
        key.WithKeys("q", "ctrl+c"),
        key.WithHelp("q", "quit"),
    ),
    Help: key.NewBinding(
        key.WithKeys("?"),
        key.WithHelp("?", "toggle help"),
    ),
}

func (k keyMap) ShortHelp() []key.Binding {
    return []key.Binding{k.Help, k.Quit}
}

func (k keyMap) FullHelp() [][]key.Binding {
    return [][]key.Binding{
        {k.Up, k.Down, k.Enter},
        {k.Help, k.Quit},
    }
}

// List item
type item struct {
    title, desc string
}

func (i item) Title() string       { return i.title }
func (i item) Description() string { return i.desc }
func (i item) FilterValue() string { return i.title }

// Model
type model struct {
    list     list.Model
    input    textinput.Model
    help     help.Model
    keys     keyMap
    showHelp bool
    status   string
}

func (m model) Init() tea.Cmd {
    return tea.EnterAltScreen
}

func (m model) Update(msg tea.Msg) (tea.Model, tea.Cmd) {
    switch msg := msg.(type) {
    case tea.WindowSizeMsg:
        m.list.SetWidth(msg.Width)
        return m, nil

    case tea.KeyMsg:
        switch {
        case key.Matches(msg, m.keys.Quit):
            return m, tea.Quit

        case key.Matches(msg, m.keys.Help):
            m.showHelp = !m.showHelp
            return m, nil

        case key.Matches(msg, m.keys.Enter):
            i, ok := m.list.SelectedItem().(item)
            if ok {
                m.status = fmt.Sprintf("Selected: %s", i.title)
            }
            return m, nil
        }
    }

    var cmd tea.Cmd
    m.list, cmd = m.list.Update(msg)
    return m, cmd
}

func (m model) View() string {
    var sections []string

    // Title
    title := titleStyle.Render(">� Bubble Tea Demo")
    sections = append(sections, title)

    // Main content
    if m.showHelp {
        sections = append(sections, m.help.View(m.keys))
    } else {
        sections = append(sections, m.list.View())
    }

    // Status bar
    if m.status != "" {
        sections = append(sections, statusStyle.Render(m.status))
    }

    // Help
    helpView := m.help.ShortHelpView([]key.Binding{
        m.keys.Help,
        m.keys.Quit,
    })
    sections = append(sections, helpStyle.Render(helpView))

    return lipgloss.JoinVertical(lipgloss.Left, sections...)
}

func initialModel() model {
    items := []list.Item{
        item{title: "Task 1", desc: "Complete the first task"},
        item{title: "Task 2", desc: "Work on second task"},
        item{title: "Task 3", desc: "Review and finalize"},
    }

    const defaultWidth = 20
    const listHeight = 14

    l := list.New(items, list.NewDefaultDelegate(), defaultWidth, listHeight)
    l.Title = "My Tasks"
    l.SetShowStatusBar(false)
    l.SetFilteringEnabled(false)
    l.Styles.Title = titleStyle
    l.Styles.PaginationStyle = helpStyle
    l.Styles.HelpStyle = helpStyle

    ti := textinput.New()
    ti.Placeholder = "Enter task..."
    ti.Focus()

    return model{
        list:     l,
        input:    ti,
        help:     help.New(),
        keys:     keys,
        showHelp: false,
    }
}

func main() {
    p := tea.NewProgram(initialModel(), tea.WithAltScreen())
    if _, err := p.Run(); err != nil {
        fmt.Printf("Error: %v", err)
        os.Exit(1)
    }
}
```

## Advanced Components

### Spinner Component
```go
import "github.com/charmbracelet/bubbles/spinner"

type model struct {
    spinner spinner.Model
}

func (m model) Init() tea.Cmd {
    return m.spinner.Tick
}

func (m model) Update(msg tea.Msg) (tea.Model, tea.Cmd) {
    var cmd tea.Cmd
    m.spinner, cmd = m.spinner.Update(msg)
    return m, cmd
}

func (m model) View() string {
    return fmt.Sprintf("%s Loading...", m.spinner.View())
}
```

### Progress Bar Component
```go
import "github.com/charmbracelet/bubbles/progress"

type model struct {
    progress progress.Model
    percent  float64
}

func (m model) View() string {
    return m.progress.ViewAs(m.percent)
}
```

### Table Component
```go
import "github.com/charmbracelet/bubbles/table"

func createTable() table.Model {
    columns := []table.Column{
        {Title: "Rank", Width: 4},
        {Title: "City", Width: 10},
        {Title: "Country", Width: 10},
    }

    rows := []table.Row{
        {"1", "Tokyo", "Japan"},
        {"2", "Delhi", "India"},
        {"3", "Shanghai", "China"},
    }

    t := table.New(
        table.WithColumns(columns),
        table.WithRows(rows),
        table.WithFocused(true),
        table.WithHeight(7),
    )

    return t
}
```

## Styling Best Practices

### Color Schemes
```go
// Define color palette
var (
    primaryColor   = lipgloss.Color("#7D56F4")
    secondaryColor = lipgloss.Color("#FF5F87")
    accentColor    = lipgloss.Color("#EE6FF8")
    bgColor        = lipgloss.Color("#1A1A1A")
    textColor      = lipgloss.Color("#FAFAFA")
    mutedColor     = lipgloss.Color("#626262")
)
```

### Layout Utilities
```go
// Responsive layout
func (m model) View() string {
    header := lipgloss.NewStyle().
        Align(lipgloss.Center).
        Width(m.width).
        Render("Header")
    
    content := lipgloss.NewStyle().
        Width(m.width).
        Height(m.height - 4).
        Render("Content")
    
    footer := lipgloss.NewStyle().
        Align(lipgloss.Center).
        Width(m.width).
        Render("Footer")
    
    return lipgloss.JoinVertical(lipgloss.Top, header, content, footer)
}
```

### Border Styles
```go
var boxStyle = lipgloss.NewStyle().
    Border(lipgloss.RoundedBorder()).
    BorderForeground(lipgloss.Color("240")).
    Padding(1, 2)
```

## Development Tips

### Debugging
```go
// Add to main function for file logging
tea.LogToFile("debug.log", "debug")

// In another terminal:
tail -f debug.log
```

### Window Size Handling
```go
case tea.WindowSizeMsg:
    m.width = msg.Width
    m.height = msg.Height
    m.list.SetSize(msg.Width, msg.Height-4)
    return m, nil
```

### Key Binding Management
```go
// Create structured key bindings
type keyMap struct {
    Up    key.Binding
    Down  key.Binding
    Enter key.Binding
    Quit  key.Binding
}

// Implement help interface
func (k keyMap) ShortHelp() []key.Binding {
    return []key.Binding{k.Up, k.Down, k.Enter, k.Quit}
}
```

## Build and Run

```bash
go build -o tui-app
./tui-app
```

## Project Structure
```
my-tui-app/
   main.go
   go.mod
   go.sum
   components/
      list.go
      input.go
      status.go
   styles/
      theme.go
   utils/
       helpers.go
```

## Common Gotchas & Solutions

### 1. Terminal Lifecycle Management
```go
// GOTCHA: Terminal left in inconsistent state
// SOLUTION: Proper cleanup with signal handling
func main() {
    p := tea.NewProgram(initialModel(), tea.WithAltScreen())
    
    // Setup graceful shutdown
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, os.Interrupt, syscall.SIGTERM)
    
    done := make(chan error, 1)
    go func() {
        _, err := p.Run()
        done <- err
    }()
    
    select {
    case err := <-done:
        if err != nil {
            log.Fatal("TUI error:", err)
        }
    case <-sigChan:
        p.Quit()
        <-done // Wait for cleanup
    }
}
```

### 2. ANSI Color Preservation
```go
// GOTCHA: Lipgloss strips ANSI codes from process output
// SOLUTION: Style only UI elements, preserve raw content
func renderLogLine(timestamp, processName, content string) string {
    // Apply style only to prefix
    prefix := fmt.Sprintf("[%s] %s: ", timestamp, processName)
    styledPrefix := logStyle.Render(prefix)
    
    // Keep content raw (preserves ANSI codes)
    return styledPrefix + content
}
```

### 3. Window Sizing Edge Cases
```go
// GOTCHA: Layout breaks with unknown dimensions
// SOLUTION: Provide defaults and minimum constraints
func (m model) Update(msg tea.Msg) (tea.Model, tea.Cmd) {
    switch msg := msg.(type) {
    case tea.WindowSizeMsg:
        // Use defaults if dimensions not set
        if msg.Width == 0 {
            msg.Width = 80
        }
        if msg.Height == 0 {
            msg.Height = 30
        }
        
        // Reserve space for UI elements
        availableHeight := msg.Height - 8 // title, tabs, help, status
        if availableHeight < 5 {
            availableHeight = 5 // minimum
        }
        
        m.width = msg.Width
        m.height = msg.Height
        m.list.SetSize(msg.Width, availableHeight)
    }
    return m, nil
}
```

### 4. Concurrency Race Conditions
```go
// GOTCHA: Race conditions with external goroutines
// SOLUTION: Use message passing and proper synchronization
type model struct {
    mu        sync.RWMutex
    processes map[string]*Process
}

func (m *model) safeAddProcess(id string, proc *Process) {
    m.mu.Lock()
    defer m.mu.Unlock()
    m.processes[id] = proc
}

// Send updates through tea.Cmd
func (m model) startProcess(id string) tea.Cmd {
    return tea.Tick(time.Millisecond*100, func(t time.Time) tea.Msg {
        return ProcessUpdateMsg{ID: id, Status: "running"}
    })
}
```

### 5. Array Field Validation
```go
// GOTCHA: Empty required arrays omitted instead of sending []
// SOLUTION: Distinguish required vs optional arrays
func validateArrayField(field FormField, value string) interface{} {
    if strings.TrimSpace(value) == "" {
        if field.Required {
            return []interface{}{} // Send empty array for required fields
        }
        return nil // Omit optional empty arrays
    }
    
    // Parse non-empty arrays
    return parseArrayValue(value)
}
```

### 6. Platform-Specific Signal Handling
```go
//go:build !windows
func setupSignalHandling(sigChan chan os.Signal) {
    signal.Notify(sigChan, os.Interrupt, syscall.SIGTERM)
}

//go:build windows
func setupSignalHandling(sigChan chan os.Signal) {
    signal.Notify(sigChan, os.Interrupt) // Windows only supports Interrupt
}
```

### 7. Non-blocking Operations
```go
// GOTCHA: Synchronous operations block UI
// SOLUTION: Use buffered channels and async processing
type LogStore struct {
    logChan chan LogEntry
    buffer  []LogEntry
    mu      sync.RWMutex
}

func (ls *LogStore) Add(entry LogEntry) {
    select {
    case ls.logChan <- entry:
        // Non-blocking send
    default:
        // Fallback to direct storage if channel full
        ls.mu.Lock()
        ls.buffer = append(ls.buffer, entry)
        ls.mu.Unlock()
    }
}
```

## Configuration Tips

- Use `tea.WithAltScreen()` for full-screen apps
- Implement proper window resize handling with defaults
- Add comprehensive key bindings with help
- Use consistent color schemes with Lipgloss
- Structure components for reusability
- Add proper error handling and recovery
- Always implement graceful shutdown with signal handling
- Use message passing over shared state for concurrency
- Preserve ANSI codes in process output
- Test for race conditions and resource leaks