---
name: personas
description: Create, maintain, and reference user personas grounded in real data. Use when any skill encounters an unknown user type, or when you need to understand who you're building for.
---

# Personas

Create, maintain, and reference user personas for your product. Personas are grounded in real data — web research on the role combined with internal knowledge from the user.

## Context Loading

Before asking anything, silently:
1. Read all files in `docs/personas/` to know which personas already exist
2. Note the `Last updated` date on each — if any are older than 3 months, you'll flag them

## Entry Point

Ask the user: **"What would you like to do?"**
- **Create a new persona** — proceed to the Creation Flow
- **Update an existing persona** — load it, ask what changed, update it
- **List existing personas** — show a summary of all personas with their key attributes and last updated dates. Flag any that are stale (>3 months old) and offer to refresh.

## Creation Flow

Ask these questions **one at a time, conversationally**. Do not present them as a checklist. Wait for each answer before asking the next. Adapt based on what the user tells you.

### Required information (completeness gate)

You need all of these before producing the persona. Track what you have and what's missing. Tell the user what's still needed.

1. **Role/title and organization type** — "Who is this person? What's their job title, and what kind of organization do they work in?"
2. **Daily responsibilities and workflows** — "Walk me through their typical day. What are they doing hour by hour?"
3. **Tools they use** — "What tools are in their daily stack? SIEM, SOAR, ticketing systems, network tools?"
4. **Pain points and frustrations** — "What makes their job hard? What frustrates them most?"
5. **Goals and success criteria** — "What does success look like for them? How is their performance measured?"
6. **Relationship with Clear NDR** — "How do they interact with Clear NDR specifically? What do they use it for? What do they wish it did better?"
7. **Technical skill level** — "How technical are they? Can they write Suricata rules? Do they read raw packets? Or are they more dashboard-oriented?"

### Web research

After the first 1-2 questions establish the role, dispatch the **market-researcher agent** to research this role:

Use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a prompt like:
> "Research the role of [role title] in [organization type]. What are their typical responsibilities, tools, pain points, career path, and how they interact with NDR/network security tools? Focus on the cybersecurity/SOC context."

If the agent reports that web tools are unavailable, skip web research and ask the user directly: *"I wasn't able to do web research on this role. Can you describe what you know about their typical responsibilities, tools, and pain points from your experience working with them?"*

Present the research findings (if available) to the user and ask them to validate: *"Here's what I found about this role from industry sources. Does this match your experience, or is your reality different?"*

### Overlap detection

If during the conversation the described persona significantly overlaps with an existing one in `docs/personas/`, flag it: *"This sounds very similar to [existing persona]. Should we update that one instead, or is this genuinely a different user type?"*

## Output

Write the persona to `docs/personas/<role-slug>.md` using this format:

```markdown
# [Role Name]

## Who they are
[Role, title, organization type, reporting structure, team context]

## Their day-to-day
[Typical workflow, what they spend time on, their rhythm]

## Pain points
- [Pain point 1]
- [Pain point 2]
- [Pain point 3]

## Goals
- [What success looks like]
- [How they're measured]

## Relationship with Clear NDR
[How they use it, what they value, what they wish was different]

## Quotes
<!-- Real quotes from user research, or synthesized from research findings -->
- "[Quote that captures their perspective]"
- "[Quote that captures a frustration]"

## Last updated: YYYY-MM-DD
```

After writing, confirm with the user: *"Here's the persona I've created. Want to adjust anything?"*

## Contextual Handoffs

- If the user mentions a competitor during persona creation → *"You mentioned [competitor]. We don't have a profile for them yet. Want to run a competitor analysis after we finish this persona?"*
- If the persona reveals an unresearched problem area → *"This pain point sounds like it could be worth investigating. Want to run a discovery session on it after we're done?"*

## Staleness

When listing or loading personas, check `Last updated` dates. If older than 3 months: *"The [persona name] profile was last updated [date]. Things may have changed — want to refresh it?"*
