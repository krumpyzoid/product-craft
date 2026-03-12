---
name: market-research
description: Research industry trends, market dynamics, and the NDR/network security landscape. Use when you need evidence to inform strategic decisions or when a skill encounters an unfamiliar market area.
---

# Market Research

Research industry trends, market dynamics, and where the NDR/network security space is heading. Produces evidence-based research briefs that inform product strategy, bet shaping, and competitive positioning.

## Context Loading

Before asking anything, silently:
1. Read all files in `docs/research/` to know what research already exists
2. Read `docs/strategy/product-strategy.md` if it exists, to understand current strategic context
3. Read `docs/competitors/` to know the competitive landscape context

## Entry Point

Ask: **"What are you trying to understand?"**

Then follow up with: **"What triggered this? A customer conversation? A competitor move? Something you read? A strategic question?"**

## Research Flow

Ask these questions **one at a time, conversationally**.

### Required information (completeness gate)

1. **Research question** — What specifically do you want to know? Help the user sharpen vague questions into researchable ones. "What's happening with AI in security" → "How are NDR vendors adopting ML-based behavioral analysis, and what detection use cases are customers seeing value from?"
2. **Scope** — "Do you want a broad landscape view or a deep dive on a specific topic?"
3. **Existing assumptions** — "What do you already know or suspect? I want to validate or challenge your assumptions, not just confirm them."

### Overlap check

Before starting research, check if `docs/research/` already has a brief covering this topic. If so: *"We already have research on [topic] from [date]. Want to build on that, or is this a different angle?"*

### Web research

Dispatch the **market-researcher agent**:

Use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a prompt like:
> "Research [specific question] in the context of the NDR/network security market. Look for: analyst reports and market forecasts, vendor announcements and positioning, industry blog posts and conference themes, technology trends, adoption patterns. Focus on sources from the last 12 months."

### Graceful degradation

If the agent reports that web tools are unavailable, inform the user: *"Web research isn't available right now. I'll need you to provide the information instead — what have you read, heard, or observed about [topic]? Think about analyst reports, conference talks, industry discussions, and what peers or customers have told you."*

Note in the output that findings are based on user-provided information, not web research.

### Synthesis and validation

After receiving research results:
1. Present key findings to the user
2. Explicitly call out where data is thin: *"I couldn't find much on [aspect]. Do you have internal data or have you heard anything from customers/partners?"*
3. Flag conflicting information: *"Source A says X but Source B says Y. What's your read?"*
4. Challenge the user's initial assumptions if the research contradicts them: *"You assumed [X], but the research suggests [Y]. What do you think?"*

## Output

Write to `docs/research/YYYY-MM-DD-<topic-slug>.md`:

```markdown
# [Topic]

## Research question
[The specific question this brief answers]

## Key findings
- [Finding 1]
- [Finding 2]
- [Finding 3]

## Evidence
### [Sub-topic 1]
[Detailed findings with source citations]

### [Sub-topic 2]
[Detailed findings with source citations]

## What this means for Clear NDR
[Implications for our product — opportunities, threats, gaps]

## Open questions
- [What we still don't know]
- [Where data was thin or conflicting]

## Recommended next steps
- [Action 1 — e.g., deeper research on X, discovery session on Y, competitor analysis on Z]

## Sources
- [URL] — [description, date]
- [URL] — [description, date]

## Last updated: YYYY-MM-DD
```

## Contextual Handoffs

- If research reveals an unserved persona → *"This research mentions [user type] as an underserved segment. We don't have a persona for them. Want to create one?"*
- If findings suggest a strategic opportunity → *"This could shift our product strategy. Want to review the strategy doc?"*
- If a specific competitor keeps appearing → *"[Competitor] keeps coming up in this research. Want to do a deep competitor analysis?"*
- If findings validate a problem worth solving → *"This looks like a real problem. Want to run a discovery session to validate it further?"*
