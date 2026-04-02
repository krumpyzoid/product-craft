---
name: competitor-analysis
description: Build and maintain deep competitor profiles with web research and internal knowledge. Use when a competitor is mentioned without an existing profile, or when evaluating competitive positioning.
---

# Competitor Analysis

Build and maintain deep competitor profiles. Combines web research with internal knowledge from sales, support, and customer conversations.

## Context Loading

Before asking anything, silently:
1. Read `product-craft.json` from the project root. If it does not exist, follow the Auto-Detection flow from the output-layer skill before proceeding.
2. Use the output layer to list all artifacts of type `competitor` — this reads from the configured destination.
3. Use the output layer to read the artifact `strategy:product-strategy` if it exists, to understand current positioning.
4. Note `Last updated` dates — flag profiles older than 3 months.

## Entry Point

Ask the user: **"What would you like to do?"**
- **New competitor profile** — "Which competitor? Or describe what you've seen and I'll help identify them."
- **Update existing profile** — show existing profiles with last updated dates, ask which to update
- **Compare competitors** — "Which two competitors do you want to compare, and on what dimensions?"

## New Profile Flow

Ask these questions **one at a time, conversationally**. Adapt based on what the user knows.

### Required information (completeness gate)

1. **Which competitor** — "Who are we looking at?" (if not already clear)
2. **Focus area** — "Do you want a full profile, or are you focused on something specific? (e.g., their detection capabilities, pricing model, how they sell against us)"
3. **Your experience competing** — "How does Clear NDR currently compete against them? Where do we win, where do we lose?"
4. **Sales intelligence** — "What do your sales reps hear in deals against them? What do customers who evaluated both say?"

### Web research

After establishing which competitor and focus area, dispatch the **market-researcher agent**:

Use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a prompt like:
> "Research [competitor name] in the NDR/network security space. Find: their product lineup and key features, target market, pricing model (if public), recent announcements/acquisitions, how they position themselves, technology approach (AI/ML, signatures, behavioral, etc.), and any analyst coverage. Check their website at [URL if known]."

If the agent reports that web tools are unavailable, skip web research and ask the user to provide the information directly: *"I wasn't able to research [competitor] online. What can you tell me about their product, positioning, and how they compete against us?"*

Present findings (if available) to the user: *"Here's what I found. What matches your experience? What's missing or wrong?"*

### Internal validation questions

After web research, fill gaps with internal knowledge:
- *"When you lose a deal to [competitor], what's usually the reason?"*
- *"When you WIN against [competitor], what tips the scale?"*
- *"Have any customers switched from [competitor] to Clear NDR? What did they say about the transition?"*
- *"What's [competitor]'s reputation in the market? What do analysts or peers say about them?"*

## Output

Produce the profile as a structured artifact block:

```
[ARTIFACT type:competitor slug:<competitor-slug> title:"<Competitor Name>"]
[SECTION "Overview"]
Who they are, founding, market position, key facts
[SECTION "Product & Technology"]
What they sell, how it works, detection approach, deployment model
[SECTION "Target market"]
Who they sell to, verticals, company sizes
[SECTION "Pricing model"]
How they charge — per endpoint, per GB, flat rate, etc. Note if public or estimated
[SECTION "Strengths (from their perspective)"]
- What they're genuinely good at
- What their marketing emphasizes
[SECTION "Weaknesses (from our perspective + market evidence)"]
- Where they fall short
- What customers complain about
[SECTION "How they position against us"]
How they differentiate from us in their messaging
[SECTION "How we win against them"]
- Competitive advantage 1
- Competitive advantage 2
[SECTION "How we lose against them"]
- Where they beat us
- What we're missing
[SECTION "Recent moves"]
- Acquisition, partnership, product launch, funding — with dates
[/ARTIFACT]
```

The output layer writes this to the configured destination.

### Comparison output

For comparisons, produce a structured artifact block:

```
[ARTIFACT type:comparison slug:<a>-vs-<b> title:"Comparison: <A> vs <B>"]
[REF competitor:<a-slug>]
[REF competitor:<b-slug>]
[SECTION "Comparison"]
| Dimension | <A> | <B> | Clear NDR |
|-----------|-----|-----|-----------|
| Detection approach | | | |
| Deployment model | | | |
| Pricing | | | |
| Target market | | | |
| Key strength | | | |
| Key weakness | | | |
[SECTION "Analysis"]
Where each has an advantage, and what this means for Clear NDR's positioning
[/ARTIFACT]
```

## Contextual Handoffs

- If a competitor feature reveals an unmet user need → *"[Competitor] offers [feature] that we don't have. This might be a pain point worth investigating. Want to run a discovery session?"*
- If the comparison reveals a strategic gap → *"There's a clear gap in [area] where both competitors beat us. This could be worth shaping into a bet. Want to start shaping?"*
- If a new user type is mentioned (e.g., "MSSPs love their multi-tenant dashboard") and no persona exists → *"We don't have a persona for [user type] yet. Want to create one?"*

## Staleness

When loading profiles, check `Last updated`. If older than 3 months: *"The [competitor] profile is from [date]. The competitive landscape moves fast — want to refresh it?"*
