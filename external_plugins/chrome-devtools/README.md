# Chrome DevTools Plugin

Browser automation, debugging, accessibility testing, and performance analysis using Chrome DevTools Protocol.

## What This Plugin Does

Enables Claude to control Chrome/Chromium browsers for:

- **Browser automation**: Navigate, click, fill forms, take screenshots
- **Debugging**: Inspect DOM, monitor console, analyze network requests
- **Accessibility testing**: WCAG compliance checks with specialized agent
- **Performance analysis**: Core Web Vitals, traces, and bottleneck identification with specialized agent

## Installation

1. Launch Chrome with remote debugging:

   **macOS:**

   ```bash
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
   ```

   **Linux:**

   ```bash
   google-chrome --remote-debugging-port=9222
   ```

   **Windows:**

   ```bash
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
   ```

2. Install the plugin:

   ```bash
   claude plugin install chrome-devtools
   ```

## Configuration

The plugin connects to Chrome DevTools using environment variables with default values:

- `DEVTOOLS_BASE_URL`: The host address (default: `127.0.0.1`)
- `DEVTOOLS_PORT`: The debugging port (default: `9222`)

To use a custom host or port, set these environment variables before starting Claude Code:

**macOS/Linux:**

```bash
export DEVTOOLS_BASE_URL=192.168.1.100
export DEVTOOLS_PORT=9223
```

**Windows (PowerShell):**

```powershell
$env:DEVTOOLS_BASE_URL="192.168.1.100"
$env:DEVTOOLS_PORT="9223"
```

**Windows (Command Prompt):**

```cmd
set DEVTOOLS_BASE_URL=192.168.1.100
set DEVTOOLS_PORT=9223
```

## Specialized Agents

### Accessibility Agent

Analyzes web applications for WCAG 2.1 AA compliance:

- Semantic HTML validation
- Keyboard navigation testing
- Screen reader compatibility
- Color contrast verification

Invoke with: `Use the web-accessibility agent to check this page`

### Performance Agent

Identifies performance bottlenecks and optimizes Core Web Vitals:

- Network request analysis
- Performance trace recording
- LCP, CLS, INP measurement
- Actionable optimization recommendations

Invoke with: `Use the web-performance agent to analyze this page`

## Usage Examples

**Accessibility testing:**

```text
"Open https://example.com and check for accessibility issues"
```

**Performance analysis:**

```text
"Run a performance trace on https://myapp.com"
```

**Browser automation:**

```text
"Fill out the contact form and submit it"
```

**Debug network issues:**

```text
"What network requests are failing on this page?"
```

## Resources

- [Chrome DevTools MCP Repository](https://github.com/ChromeDevTools/chrome-devtools-mcp/)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)
