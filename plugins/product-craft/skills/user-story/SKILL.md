---
name: user-story
description: Write user stories that express need and impact from the user's perspective. Refuses to write stories in technical terms. Use when scopes in a bet need stories, or when capturing user needs.
---

# User Story

Write user stories that express need and impact from the user's perspective, not technical implementation. This skill is relentless about user-perspective framing — it will push back on technical language every time.

## The Rule

**No technical implementation in user stories.** "As a user, I want an API endpoint" is not a user story. "As a SOC analyst, I want to see which hosts communicated with a known C2 server in the last 24 hours so I can prioritize my investigation" is a user story.

## Context Loading

Before asking anything, silently:
1. Read `product-craft.json` from the project root. If it does not exist, follow the Auto-Detection flow from the output-layer skill before proceeding.
2. Use the output layer to list all artifacts of type `persona`.
3. Use the output layer to list all artifacts of type `research` (discovery briefs and evaluations) for problem context.
4. If invoked from `/shape-bet`, note which pitch this story belongs to.

## Entry Point

Present existing personas: *"Which persona is this story for?"*

List the personas loaded during Context Loading with a one-line summary of each. If the user names someone that doesn't match an existing persona, offer to create one: *"We don't have a persona for [user type]. Want to define one first? It'll make the story stronger."*

## Story Building Flow

Ask questions **one at a time, conversationally**. Build the story incrementally.

### Required information (completeness gate)

1. **Persona** — Which specific persona? (from entry point)
2. **Problem context** — "What problem does this story address?" Check if a discovery brief or evaluation exists among the research artifacts loaded during Context Loading. If so, reference it. If not: *"We don't have a validated problem for this yet. Is this based on a known pain point, or should we validate first?"*
3. **User goal** — "What is this person trying to accomplish? Not what feature they want — what are they trying to DO?"
4. **Impact** — "When this works, what changes for them? How is their day better?"
5. **Current state** — "What do they do today without this? What's the workaround or pain?"
6. **Acceptance criteria** — "How would this person know it's working? What would they observe?"

### User-perspective enforcement

**When the user uses technical language:**
> User: "As a user, I want to query the API for detection events"
> You: "That's implementation. What is the analyst trying to find out? What question are they answering when they look at detection events?"

> User: "Parse Zeek logs and correlate with Suricata alerts"
> You: "What will the analyst be able to see or do that they can't today? Let's write this from their perspective."

> User: "Implement WebSocket notifications for real-time alerts"
> You: "What experience are we creating? Something like 'As a SOC analyst, I want to be notified immediately when a critical threat is detected so I can respond before the attacker moves laterally'?"

Keep redirecting until the story is in the persona's language, not the developer's.

### Acceptance criteria enforcement

Acceptance criteria must be **observable outcomes**, not implementation details:
- BAD: "API returns JSON with status 200"
- GOOD: "The analyst can see the threat timeline within 3 clicks of receiving an alert"
- BAD: "Database query completes in under 500ms"
- GOOD: "Search results appear fast enough that the analyst's workflow isn't interrupted"

## Output

### Standalone stories

Produce the story as a structured artifact block:

```
[ARTIFACT type:story slug:<title-slug> title:"User Story: <Short title>" date:<YYYY-MM-DD>]
[REF persona:<persona-slug>]
[REF research:<brief-slug>]
[SECTION "Story"]
As a [persona name and role],
I want to [goal in their language]
so that [impact on their work].
[SECTION "Acceptance criteria"]
- [ ] Observable outcome from the user's perspective
- [ ] Observable outcome from the user's perspective
[SECTION "What they do today"]
Current workaround or pain — how they cope without this
[SECTION "What changes for them"]
The "after" — how their experience improves, in concrete terms
[/ARTIFACT]
```

The output layer writes this to the configured destination. For local files, this renders to `{basePath}/pitches/stories/{date}-{slug}.md`.

### Stories within a pitch

When invoked from `/shape-bet`, produce stories in the same format but embed them in the pitch under the relevant scope section rather than writing standalone files.

## Contextual Handoffs

- No matching persona → *"We need a persona for this story. Want to create one now?"* → offers `/personas`
- No validated problem → *"This story isn't grounded in a validated problem. Want to run a quick discovery to strengthen it?"* → offers `/discovery`
- Multiple related stories emerging → *"These stories are forming a coherent scope. Want to shape them into a bet?"* → offers `/shape-bet`
