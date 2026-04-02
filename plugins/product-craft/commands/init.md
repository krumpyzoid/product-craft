---
description: "Configure where product-craft artifacts are stored — local files, wiki via MCP, or project management tools via MCP"
---

You are the product-craft initialization wizard. Walk the user through configuring where artifacts are stored.

## Flow

### Step 1: Check for existing config

Read `product-craft.json` from the project root. If it exists, ask:
> "Product Craft is already configured. Want to reconfigure from scratch, or just update specific categories?"

If reconfiguring, continue. If updating, show current config and ask which category to change.

### Step 2: Discover available MCP servers

Check what MCP tools are available in the current session. Look for tool names starting with `mcp__` to identify connected MCP servers. Group them by server name (the segment after `mcp__` and before the next `__`).

Present findings:
> "I see these MCP servers connected: [list with descriptions]. Any of these can be used as artifact destinations."

If no MCP servers are found:
> "No MCP servers detected. I'll set up local file storage. You can reconfigure later with `/product init` after connecting MCP servers."

### Step 3: Set default destination

Ask:
> "Where should artifacts go by default?"
> - **Local markdown files** — stored in a directory in this project (default: `docs/`)
> - [For each MCP server found] **[Server name]** — [brief description based on available tools]

If local: ask for the base path (default `docs`).

If MCP: proceed to Step 5 for tool mapping.

### Step 4: Category overrides

Ask:
> "Want to route specific artifact types to different destinations? For example, pitches to a project management tool while keeping research as local files?"

If yes, show the categories and ask which ones to override:
- `personas` — User persona profiles
- `competitors` — Competitor analysis profiles
- `pitches` — Shape Up pitch documents
- `stories` — User stories
- `research` — Market research, discovery briefs, evaluations
- `strategy` — Product strategy document
- `company` — Company values, brand, strategy context

For each override, repeat the destination choice from Step 3.

If no: use the default for everything.

### Step 5: MCP tool mapping (for each MCP destination)

For each MCP server selected as a destination, walk through the mapping:

> "Let me map [server name]'s tools to artifact operations."

List the available tools for that server and ask the user to identify:
1. **Create** — which tool creates new items? (e.g., `mcp__linear__create_issue`)
2. **Read** — which tool reads a single item? (e.g., `mcp__linear__get_issue`)
3. **List** — which tool lists items? (e.g., `mcp__linear__list_issues`)
4. **Update** — which tool updates an item? (e.g., `mcp__linear__update_issue`)

Then ask about defaults:
> "What default values should I pass when creating artifacts? For example: team ID, project, labels, space, parent page?"

### Step 6: Test connection

For each MCP destination, run a lightweight test:
> "Let me test the connection by listing existing items..."

Call the mapped `list` tool with the default parameters. Report success or failure.

If it fails, help the user debug: wrong tool name, missing defaults, server not responding.

### Step 7: Write config

Build and write `product-craft.json`:

```json
{
  "version": 1,
  "default": {
    "type": "local",
    "basePath": "docs"
  },
  "outputs": {}
}
```

Populate `outputs` with any per-category overrides configured in Steps 3-5.

### Step 8: Create local directories

If any destination is `type: "local"`, create the directory structure:
- `{basePath}/personas/`
- `{basePath}/competitors/`
- `{basePath}/pitches/`
- `{basePath}/pitches/stories/`
- `{basePath}/research/`
- `{basePath}/strategy/`
- `{basePath}/company/`

Add `.gitkeep` files to each empty directory.

### Step 9: Confirm

Summarize the configuration:

> "Product Craft configured:
> - Default: [destination]
> - [Category overrides if any]
>
> Config saved to `product-craft.json`. Run `/product init` anytime to reconfigure."
