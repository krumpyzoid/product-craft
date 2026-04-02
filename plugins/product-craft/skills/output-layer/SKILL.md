---
name: output-layer
description: "Internal skill for artifact I/O. Reads config, writes/reads artifacts to configured destinations (local files or MCP). Not user-facing — invoked by other skills via conventions."
---

# Output Layer

You are the artifact I/O layer for product-craft. Other skills produce structured artifact blocks. You read the config, resolve destinations, and handle reading/writing artifacts.

**This skill is not invoked directly by users.** Other skills embed its conventions.

## Configuration

Read `product-craft.json` from the project root. If the file does not exist, see the Auto-Detection section below.

### Config structure

The config maps artifact categories to output destinations:

- `default` — fallback for any category not explicitly overridden
- `outputs` — per-category overrides
- `type: "local"` — write markdown files to `{basePath}/{category}/`
- `type: "mcp"` — route through an MCP server using the mapped tools

### Category mapping

Artifact types map to config categories:

| Artifact type | Config category |
|---------------|-----------------|
| persona | personas |
| competitor | competitors |
| comparison | competitors |
| pitch | pitches |
| story | stories |
| research | research |
| discovery | research |
| evaluation | research |
| strategy | strategy |

## Auto-Detection

When any skill starts and no `product-craft.json` exists:

Tell the user:
> "Product Craft hasn't been configured yet. Would you like to set up where artifacts are stored? (Run `/product setup` for full setup, or I can use local markdown files in `docs/` as the default.)"

If the user picks the quick default, write this to `product-craft.json`:

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

Then create the directory structure `{basePath}/personas/`, `{basePath}/competitors/`, `{basePath}/pitches/`, `{basePath}/pitches/stories/`, `{basePath}/research/`, `{basePath}/strategy/`, `{basePath}/company/` with `.gitkeep` files, and continue with the skill.

If the user wants full setup, invoke the `product-craft:setup` command skill and wait for it to complete before continuing.

## Artifact Block Format

Skills produce artifacts using this structured block format:

```
[ARTIFACT type:<type> slug:<slug> title:"<title>" date:<YYYY-MM-DD> status:<status>]
[META key:value key2:value2]
[SECTION "Section Name"]
Content of this section...
[SECTION "Another Section"]
More content...
[REF type:slug]
[REF type:slug]
[/ARTIFACT]
```

Fields:
- `type` — required. One of: persona, competitor, comparison, pitch, story, research, discovery, evaluation, strategy
- `slug` — required. Unique identifier within type (e.g. `soc-analyst`, `darktrace`)
- `title` — required. Human-readable name
- `date` — optional. ISO date for types that use date prefixes
- `status` — optional. Lifecycle status (for pitches: Draft, Ready for Betting, Bet, Shipped)
- `[META]` — optional. Key-value pairs for MCP targets (labels, assignees, team, etc.)
- `[SECTION]` — one or more named sections forming the body
- `[REF]` — cross-references to other artifacts using type:slug notation

## Writing Artifacts

### Local files (`type: "local"`)

1. Determine file path: `{basePath}/{category}/{filename}.md`
2. Build filename from artifact fields:
   - Types with date prefix (research, discovery, evaluation, pitch, story): `{date}-{slug}.md`
   - Discovery artifacts: `discovery-{date}-{slug}.md`
   - Evaluation artifacts: `eval-{date}-{slug}.md`
   - All other types: `{slug}.md`
3. Render markdown:
   - Title becomes `# {title}`
   - Status (if present) becomes `## Status: {status}`
   - Each `[SECTION]` becomes `## {Section Name}` followed by content
   - Each `[REF type:slug]` becomes a markdown link: resolve the slug to its file path within `{basePath}` using the category mapping and filename conventions, then produce a relative link like `[Title](../category/filename.md)`
   - Add `## Last updated: {date}` at the end
4. Write the file using the Write tool

### MCP (`type: "mcp"`)

1. Look up the category's config in `outputs`
2. Build the MCP tool call parameters:
   - Title maps to the appropriate title/name/summary field
   - Concatenate all `[SECTION]` contents into a description/body field, using `## Section Name` as delimiters
   - `[META]` key-value pairs merge with the `defaults` from config and are passed as tool parameters
   - `[REF]` references: query the target system to find matching artifacts (by title or slug), then insert links in the body text
3. Call the mapped `create` tool (or `update` if the artifact already exists — check with `list` first)

## Reading Artifacts

Skills request reads using this convention:

> "Use the output layer to list all artifacts of type `{type}`" — returns a list of artifact summaries (slug, title, date, status)
> "Use the output layer to read artifact `{type}:{slug}`" — returns the full artifact in structured form

### Reading from local files (`type: "local"`)

1. Determine directory: `{basePath}/{category}/`
2. List or read `.md` files in that directory
3. Parse each file back into artifact structure:
   - `# Title` → title
   - `## Status: X` → status
   - `## Section Name` followed by content → `[SECTION]`
   - `## Last updated: date` → date
   - Markdown links to other artifact files → `[REF]`
4. Return structured data to the requesting skill

### Reading from MCP (`type: "mcp"`)

1. Call the mapped `list` tool (with any default filters from config)
2. For each result, normalize into artifact structure:
   - Map title/name/summary → title
   - Map description/body → parse sections if delimited, or treat as single section
   - Map tool-specific fields (labels, status, assignees) → metadata
3. Return structured data to the requesting skill

## Resolving References

When a skill includes `[REF persona:soc-analyst]`:

1. Look up where `persona` artifacts are stored (config category: `personas`)
2. If local: resolve to relative markdown link `[SOC Analyst](../personas/soc-analyst.md)`
3. If MCP: query the target system for an artifact with matching slug/title, return its URL or identifier

## Updating Artifacts

When a skill updates an existing artifact (e.g., refreshing a persona):

1. Read the existing artifact first
2. Apply changes to the relevant sections
3. Update the `Last updated` date
4. Write using the same destination (local file overwrite, or MCP `update` call)
