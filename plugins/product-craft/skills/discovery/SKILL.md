---
name: discovery
description: Validate whether a problem is real, worth solving, and for whom. Prevents jumping to solutions. Use when someone proposes a feature, reports a pain point, or when a competitor move triggers a question about user needs.
---

# Discovery

Validate whether a problem is real, worth solving, and for whom. This skill is deliberately confrontational — it challenges assumptions, asks for evidence, and refuses to let you skip from "idea" to "solution" without understanding the problem first.

## The Rule

**No solutions until the problem is validated.** If the user describes a feature, you reframe it as a problem. If they describe a technical approach, you ask who benefits and why. Every time.

## Context Loading

Before asking anything, silently:
1. Read all files in `docs/personas/` to know which user types are defined
2. Read all files in `docs/research/` to check for existing research on this topic
3. Read `docs/strategy/product-strategy.md` if it exists, for strategic context

## Entry Point

Ask: **"What made you think about this? A customer said something? You noticed something? Sales reported something? A competitor launched something?"**

This establishes the trigger and sets the evidence baseline.

## Discovery Flow

Ask questions **one at a time, conversationally**. Be curious but rigorous.

### Required information (completeness gate)

Track what you have and what's missing. Tell the user explicitly: *"I still need to understand: [missing items]."*

1. **Trigger** — What started this investigation? (already asked at entry)
2. **Who has this problem** — "Which of our users experiences this?" Present existing personas from `docs/personas/`. If the user describes someone new: *"This sounds like a user type we haven't captured yet. Let's note that — we may want to create a persona for them."*
3. **Problem in user terms** — "Describe the problem from the user's perspective. Not what's technically wrong — what is the person trying to do, and what's blocking them or making it painful?"
4. **Current workaround** — "How do they cope today? Do they use another tool? A manual process? Do they just suffer through it?"
5. **Frequency and severity** — "How often does this happen? And when it does, how bad is it? Minor annoyance or blocks their work?"
6. **Evidence breadth** — "Is this one customer, or have you heard this from multiple sources? Support tickets? Sales conversations? Usage data?"
7. **What happens if we do nothing** — "If we never solve this, what's the consequence? For the user? For us?"

### Problem-first enforcement

**When the user describes a solution:**
> User: "We need to add a REST API for custom detection rules"
> You: "Let's back up — what problem would that solve? Who needs custom detection rules, and what are they trying to accomplish that they can't today?"

**When the user describes a technical issue:**
> User: "The detection engine doesn't support behavioral analysis"
> You: "What would behavioral analysis let the user do that they can't do now? Which user is asking for this, and what evidence do we have that it matters to them?"

**When the user says "competitors have it":**
> User: "Darktrace has AI-based anomaly detection and we don't"
> You: "That's about Darktrace's approach, not our users' needs. Are our customers asking for anomaly detection? What problems are they hitting that they think AI would solve? Or are we assuming?"

Be persistent but not obnoxious. If the user gives a clear problem statement, don't keep asking. But if they keep defaulting to solutions, keep redirecting.

### Web research

If the problem area has broader industry relevance, dispatch the **market-researcher agent**:

Use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a prompt like:
> "Research whether [problem area] is a recognized pain point in the NDR/SOC space. Look for: industry surveys mentioning this pain point, vendor solutions addressing it, analyst commentary, community discussions (Reddit, security forums). Is this specific to our product or an industry-wide challenge?"

If the agent reports that web tools are unavailable, ask the user directly: *"I couldn't research this online. Is this a well-known pain point in the industry, or something more specific to your users? What have you heard from customers, peers, or at conferences?"*

Present findings (if available): *"The research suggests this is [widespread/niche/emerging]. Does that match what you're seeing?"*

## Problem Scoring

After gathering all information, score the problem:

| Dimension | Rating (1-5) | Notes |
|-----------|:---:|-------|
| **Frequency** — How often does this happen? | | |
| **Severity** — How bad is it when it does? | | |
| **Breadth** — How many personas/segments affected? | | |
| **Trend** — Is this getting worse? | | |

**Scoring guidance:**
- 1 = Rare/minor/one user type/stable
- 3 = Regular/moderate/several user types/growing
- 5 = Constant/critical/all user types/accelerating

Present the score to the user: *"Based on what we've discussed, here's how I'd rate this problem. Do you agree, or would you adjust anything?"*

## Output

Write to `docs/research/discovery-YYYY-MM-DD-<topic-slug>.md`:

```markdown
# Discovery: [Problem Statement — in user terms]

## Trigger
[What started this investigation]

## Affected personas
[Links to persona files, or description of new user type if no persona exists yet]

## Problem description
[The problem from the user's perspective — what they're trying to do, what's blocking them, how it feels]

## Current workarounds
[How users cope today]

## Evidence
[All evidence gathered — customer quotes, support tickets, research findings, competitive context]

## Problem score
| Dimension | Rating (1-5) | Notes |
|-----------|:---:|-------|
| Frequency | | |
| Severity | | |
| Breadth | | |
| Trend | | |

**Total: [sum]/20**

## What happens if we don't solve this
[Consequences for users and for the business]

## Recommended next step
<!-- One of: -->
<!-- - Shape a bet (score >= 12, strong evidence) -->
<!-- - More research needed (score uncertain, weak evidence) -->
<!-- - Park it (score < 8, limited evidence) -->
<!-- - Decline (not our problem to solve) -->
[Recommendation with reasoning]

## Last updated: YYYY-MM-DD
```

## Contextual Handoffs

- If problem is validated (score >= 12, good evidence) → *"This looks like a real, significant problem. Want to shape it into a bet?"*
- If an affected user type has no persona → *"We don't have a persona for [user type] yet. Want to create one? It'll make the bet stronger."*
- If workaround involves a competitor tool → *"Users are turning to [competitor] for this. Want to do a competitor analysis to understand their approach?"*
- If more market context is needed → *"The evidence is thin on how widespread this is. Want to do broader market research?"*
