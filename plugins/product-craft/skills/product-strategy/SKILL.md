---
name: product-strategy
description: Step back from individual bets and think about the big picture. Product positioning, strategic themes, and cycle planning. Use when starting a new cycle, after market shifts, or when priorities feel unclear.
---

# Product Strategy

Step back from individual bets and think about the big picture. Where is Clear NDR heading, how do we position against competitors, what are the themes for the next cycle.

## Context Loading

Before asking anything, silently load everything:
1. `docs/strategy/product-strategy.md` — current strategy if it exists
2. `docs/competitors/` — all competitor profiles
3. `docs/personas/` — all personas
4. `docs/pitches/` — all pitches (note their statuses)
5. `docs/research/` — all research briefs and discovery briefs
6. `docs/company/core-values.md`, `docs/company/brand.md`, `docs/company/strategy.md` — company-level context
7. Check `Last updated` dates on everything — flag stale artifacts

## Entry Point

Start with what you found: *"Here's what I see in our product docs: [X] personas, [Y] competitor profiles, [Z] pitches ([N] ready for betting, [M] in draft), [W] research briefs. The strategy was last updated [date / never created]."*

Then ask: **"What's prompting this? New cycle starting? Something shifted in the market? Feeling lost on priorities?"**

## Strategy Flow

This is a high-level, conversational process. Ask questions **one at a time**.

### Required information (completeness gate)

1. **Time horizon** — "What time frame are we thinking about? Next cycle? Next 6 months? Next year?"

2. **What's changed** — "What's different since the last strategy review (or since we started)? Market shifts? Competitor moves? Customer feedback patterns? Team changes?"

3. **Current strengths** — "What's Clear NDR genuinely good at? What do customers love? What do we do better than anyone else?"

4. **Current weaknesses** — "Where do we fall short? What do customers ask for that we can't deliver? Where do competitors beat us?"

5. **Existing bets and pitches** — Review what's in `docs/pitches/`. *"We have these pitches in various stages: [list]. Do they still feel right? Any that should be dropped? Any gaps?"*

### Strategic framework

Walk through this with the user:

**Where are we now?**
- Current market position (informed by competitor profiles)
- What customers value about Clear NDR
- Team size and capability constraints
- Revenue/growth context (if shared)

**Where do we want to be?**
- Vision for Clear NDR in [time horizon]
- What should customers say about us that they don't today?
- What market position are we targeting?

**What's in our way?**
- Competitive gaps (from competitor profiles)
- Capability gaps (from team constraints)
- Market perception gaps (from brand/positioning)

**What themes emerge?**
- Group existing pitches, research, and competitive intelligence into themes
- A theme is a strategic direction, not a feature: "Close the behavioral analysis gap" or "Win the MSSP segment" or "Become the transparency leader"

### Web research (if needed)

If market context is thin, dispatch the **market-researcher agent**:

Use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a prompt like:
> "Research current NDR market trends for [time horizon]. What are analysts saying? What themes are emerging at conferences? How is the competitive landscape shifting? Focus on [specific area if applicable]."

If the agent reports that web tools are unavailable, proceed without web research. Ask the user: *"I can't do web research right now. What market trends have you been tracking? Any analyst reports, conference themes, or industry shifts you've noticed?"*

### Conflict detection

When reviewing existing pitches against the emerging themes, check for conflicts:
- Pitches that contradict the "What we're NOT doing" section → *"These pitches conflict with our stated trade-offs: [list]. Should they be parked, or do the trade-offs need updating?"*
- Pitches aligned with themes that are being dropped → *"These pitches were in a theme we're deprioritizing: [list]. What should happen to them?"*

Surface conflicts explicitly and require resolution before finalizing the strategy.

### Prioritization

Help the user prioritize ruthlessly:

*"Given your small team, you can realistically pursue 2-3 themes per cycle. Which of these matter most?"*

For each candidate theme, assess:
- **Impact** — How much does this move the needle for users and for the business?
- **Feasibility** — Can we make meaningful progress with our team and timeline?
- **Evidence** — Do we have research, discovery briefs, or customer data supporting this?
- **Urgency** — Is there a competitive or market window we need to hit?

### Deliberate trade-offs

Force explicit trade-offs:

*"If you're doing [theme 1] and [theme 2], you're NOT doing [theme 3]. Is that a conscious choice? What's the risk of not pursuing it?"*

Document what you're not doing and why. This is as important as what you are doing.

## Output

Write to or update `docs/strategy/product-strategy.md`:

```markdown
# Product Strategy

## Last updated: YYYY-MM-DD
## Time horizon: [period]

## Positioning
[How Clear NDR wins — our unique angle in the market.
Not a tagline — a clear statement of why customers choose us over alternatives.]

## Strategic themes

### Theme 1: [Name]
**Why this matters:** [Evidence-based rationale]
**Supporting evidence:** [Links to research, competitor profiles, discovery briefs]
**Key bets in this theme:**
- [Pitch title] (status) — link to docs/pitches/...
- [Pitch title] (status) — link to docs/pitches/...
**Gaps:** [What still needs to be shaped or researched]

### Theme 2: [Name]
**Why this matters:** [Evidence-based rationale]
**Supporting evidence:** [Links]
**Key bets in this theme:**
- [Pitch title] (status)
**Gaps:** [What still needs work]

### Theme 3: [Name]
...

## What we're NOT doing (and why)
- [Deliberate trade-off 1] — [Why we're choosing not to pursue this now]
- [Deliberate trade-off 2] — [Why]

## Key risks
- [Strategic risk 1] — [How we're watching it, what would change our mind]
- [Strategic risk 2] — [How we're watching it]

## Inputs
[Links to competitor profiles, research briefs, discovery briefs, and pitches that informed this strategy]
```

After writing: *"Strategy updated. Here's a summary of the themes and trade-offs. Does this capture where we're heading?"*

## Contextual Handoffs

- A theme has no supporting research → *"We don't have research backing this theme. Want to do market research on [topic]?"*
- A theme implies a persona we haven't defined → *"This theme targets [user type] but we don't have a persona for them. Want to create one?"*
- A theme needs to be shaped into a bet → *"This theme doesn't have any pitches yet. Want to shape a bet?"*
- Competitive positioning feels stale → *"Our competitor profiles are [X months] old. Want to refresh them?"*
- Existing pitches don't align with themes → *"These pitches don't fit the current themes: [list]. Should they be parked, reshaped, or do the themes need adjusting?"*

## Staleness

Flag stale inputs during context loading:
- Competitor profiles > 3 months old
- Research briefs > 6 months old
- Strategy itself > 3 months old

*"Some of our inputs are stale: [list]. Want to refresh any of these before we proceed?"*
