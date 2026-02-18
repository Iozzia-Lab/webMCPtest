# WebMCP Test Dashboard & UI Gauntlet

A comprehensive test suite for WebMCP (Web Model Context Protocol) — the new browser-native API that lets websites expose structured, callable tools to AI agents through navigator.modelContext.

This repo contains two test pages for verifying WebMCP functionality:

- **Test Dashboard** (index.html) — 6 registered WebMCP tools for a restaurant ordering flow
- **UI Gauntlet** (gauntlet.html) — 18 interactive UI sections with 16 registered WebMCP tools covering every common interaction pattern

Read the full writeup: [I Built a WebMCP Test Dashboard — Here's How the Browser's New AI API Actually Works](https://x.com/saliozzia) *(X article by @saliozzia)*

## Prerequisites — All Required, In Order

WebMCP is not just "install an MCP and go." There is a chain of prerequisites and every link must be in place. Skip one and nothing happens.

1. **Sign up for the Early Preview Program** — WebMCP is gated behind Google's Early Preview Program. Apply via the [Chrome blog post](https://developer.chrome.com/blog/webmcp-epp) and wait for approval.

2. **Install Chrome Canary (146+)** — WebMCP does not exist in regular Chrome, Chrome Beta, or any other browser. Download [Chrome Canary](https://www.google.com/chrome/canary/).

3. **Enable the WebMCP flag** — Even in Canary, WebMCP is off by default. Go to `chrome://flags/#enable-webmcp-testing`, set it to Enabled, and relaunch.

4. **Serve this repo and open it in Canary** — The pages in this repo already have WebMCP tool registrations wired up (see below). For your own site, you need to add JavaScript that calls `navigator.modelContext.registerTool()` for each tool you want to expose.

5. **Connect a working MCP server / AI harness** — Install and configure an MCP server that supports WebMCP (e.g., `@mcp-b/chrome-devtools-mcp`) so your AI agent can discover and call the tools registered in the browser tab.

## Quick Start

```bash
# Clone the repo
git clone https://github.com/Iozzia-Lab/webMCPtest.git
cd webMCPtest

# Serve the page
npx serve .
# or
python -m http.server 3000
```

Open Chrome Canary (launched with `--remote-debugging-port=9222`) and navigate to:
- `http://localhost:3000/` — Test Dashboard (6 tools)
- `http://localhost:3000/gauntlet.html` — UI Gauntlet (18 sections, 16 tools)

Verify WebMCP is active by opening DevTools console and checking that `'modelContext' in navigator` returns true. The status badge in the top-right corner of each page shows green when connected.

## Test Dashboard Tools (index.html)

| Tool | Description | Input |
|------|-------------|-------|
| `greet_user` | Personalized greeting | `{ name }` |
| `search_items` | Search mock product catalog | `{ query, category? }` |
| `toggle_theme` | Switch light/dark mode | `{ theme? }` |
| `get_page_stats` | Page state and stats | `{ }` |
| `submit_form` | Validated form submission | `{ name, email, message }` |
| `calculate` | Math expression evaluator | `{ expression }` |

## UI Gauntlet (gauntlet.html)

The gauntlet is a comprehensive interaction test covering every common UI pattern an AI agent might encounter. It registers 16 WebMCP tools across 18 collapsible sections:

- Text inputs, textareas, date pickers
- Single select, multi-select, grouped dropdowns
- Checkboxes, radio buttons, toggle switches, range sliders
- Tab navigation
- Data grid with View/Edit/Delete (CRUD actions)
- Toast notifications (success, error, warning, info)
- Alert banners with dismiss
- Modal dialogs and stacked modals
- Accordion/collapsible sections
- Drag and drop reorder
- Search with autocomplete
- Multi-step wizard
- File upload zone
- Confirmation dialogs
- Full page state introspection via `get_page_state`

The idea: if an AI agent can navigate this gauntlet entirely through WebMCP tools — no clicking, no vision — then the protocol works.

## Testing with Claude

1. Launch Chrome Canary with remote debugging on port 9222
2. Navigate to the test page
3. Open a Claude Code or Cowork session with the WebMCP MCP server configured
4. Try: "List the WebMCP tools on this page"
5. Try: "Search for pizza in the catalog"
6. Try: "Fill out the form and submit it"
7. Try: "Navigate to the Reviews tab"
8. Try: "Get the full page state"

## How It Works

Each page imports the @mcp-b/global polyfill (wrapped in error handling — it currently throws a TypeError but fails gracefully) and registers tools via navigator.modelContext.registerTool(). The @mcp-b/chrome-devtools-mcp server connects to Chrome via the DevTools Protocol, auto-discovers these tools, and exposes them as standard MCP tools that Claude can invoke.

Tool handlers return results in MCP content format: an object with a content array containing typed text entries. This is required — returning plain objects will silently fail.

## Resources

- [WebMCP Spec](https://webmcp.link)
- [Chrome Blog — WebMCP Early Preview](https://developer.chrome.com/blog/webmcp-epp)
- [X Article — Full Writeup](https://x.com/saliozzia)

## Credits

Built by [@saliozzia](https://x.com/saliozzia) — QA testing WebMCP for Ser.vi's restaurant ordering platform.

Claude Cowork — at the direction of @saliozzia