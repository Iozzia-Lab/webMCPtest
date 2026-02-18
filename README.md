# WebMCP Test Dashboard

A web page that has elements on it with webMCP registration code to allow us to test if webMCP is working correctly.

## Prerequisites

- **Chrome Canary 146+** with the "WebMCP for testing" flag enabled in `chrome://flags`
- **Node.js** (for serving the page and the MCP server)
- **@mcp-b/chrome-devtools-mcp** registered as an MCP server for Claude

## Quick Start

```bash
# 1. Install dependencies (for local dev only, polyfill loads from CDN)
npm install

# 2. Serve the page
npx serve .

# 3. Open Chrome Canary (with remote debugging) and navigate to http://localhost:3000
```

Make sure Chrome Canary is launched with `--remote-debugging-port=9222`.

## Registered Tools

| Tool | Description | Input |
|------|-------------|-------|
| `greet_user` | Personalized greeting | `{ name }` |
| `search_items` | Search mock product catalog | `{ query, category? }` |
| `toggle_theme` | Switch light/dark mode | `{ theme? }` |
| `get_page_stats` | Page state and stats | `{ }` |
| `submit_form` | Validated form submission | `{ name, email, message }` |
| `calculate` | Math expression evaluator | `{ expression }` |

## Testing with Claude

1. Launch Chrome Canary from your debug shortcut (port 9222)
2. Navigate to the test page
3. Open a Claude Code or Cowork session
4. Try: "List the WebMCP tools on this page"
5. Try: "Search for pizza in the catalog"
6. Try: "Submit a form with my details"
7. Try: "Toggle the page to light mode"

## How It Works

The page imports `@mcp-b/global` (the WebMCP polyfill) and registers tools via `navigator.modelContext.registerTool()`. The `@mcp-b/chrome-devtools-mcp` MCP server connects to Chrome via the DevTools Protocol, auto-discovers these tools, and exposes them as standard MCP tools that Claude can invoke.
