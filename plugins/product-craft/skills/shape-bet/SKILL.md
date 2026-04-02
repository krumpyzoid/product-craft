---
name: shape-bet
description: Shape a validated problem into a Shape Up pitch with clear boundaries, appetite, and risks. The core PM artifact. Use when a problem has been validated through discovery or when enough evidence exists to shape work.
---

# Shape Bet

Shape a validated problem into a Shape Up pitch. This is where everything converges — personas, discovery, competitor analysis, and strategy come together into a concrete proposal for the betting table.

Read the shape-up-primer and pitch-template references from the plugin's `references/` directory for methodology and output structure guidance.

## The Rules

1. **No pitch without a problem.** If the problem isn't clear, redirect to `/discovery`.
2. **No pitch without evidence.** "We think it's important" is not evidence.
3. **No pitch without a persona.** "Users" is not a persona.
4. **Appetite is a budget, not an estimate.** "How much is this worth?" not "How long will it take?"
5. **Scopes are user-visible.** "Refactor the parser" is not a scope. "Analysts can see encrypted traffic metadata" is a scope.

## Context Loading

Before asking anything, silently:
1. Read `product-craft.json` from the project root. If it does not exist, follow the Auto-Detection flow from the output-layer skill before proceeding.
2. Use the output layer to list all artifacts of type `persona`.
3. Use the output layer to list all artifacts of type `research` (discovery briefs and evaluations).
4. Use the output layer to list all artifacts of type `competitor`.
5. Use the output layer to read the artifact `strategy:product-strategy` if it exists.
6. Use the output layer to list all artifacts of type `pitch` to avoid duplication.
7. Read the plugin's `references/shape-up-primer.md` and `references/pitch-template.md`.

## Entry Point

Ask: **"What problem are we shaping a bet around?"**

If the user references an existing discovery brief or evaluation, load it: *"I see the discovery brief from [date]. Let me use that as our starting point."*

## Shaping Flow

Ask questions **one at a time, conversationally**. This is a rigorous process — don't rush it.

### Conflict detection

After loading the product strategy artifact, check whether the problem being shaped conflicts with the "What we're NOT doing" section. If it does, surface this as a blocker:

*"Hold on — our product strategy explicitly says we're NOT doing [X]. This bet is about [Y], which seems to conflict. Before we continue, we need to resolve this: has the strategy changed, or should this bet be reframed to avoid the conflict?"*

Do not proceed with shaping until the user explicitly resolves the conflict (either by updating the strategy or reframing the bet).

### Solution-first interception

If the user starts with a solution:
> User: "I want to shape a bet for adding ML-based anomaly detection"
> You: "That's a solution. Let's find the problem first. What problem would ML-based anomaly detection solve for our users? Who's struggling with what?"

If no discovery brief exists for the problem:
> *"We haven't validated this problem yet. Want to run a quick discovery session first, or do you have strong enough evidence to proceed?"*

If evidence is strong enough to proceed without formal discovery, note it and continue.

### Required information (completeness gate)

Track each item. Tell the user what's still missing.

1. **Problem statement** — In user terms, with evidence. Must reference a persona. Must have evidence beyond opinion.

2. **Personas affected** — Which specific personas from the artifacts loaded during Context Loading? If none match: *"This doesn't map to any of our defined personas. Either we need a new persona, or this problem isn't in our target market."*

3. **Appetite** — *"How much time is this worth to us? Not how long will it take — how much would we invest to solve this?"*
   - Guide the user: *"In Shape Up, appetite is a budget. A 1-week small batch for minor improvements, 2-week small batch for moderate changes, or 6-week big batch for significant features. If it can't fit in 6 weeks, we need to cut scope."*
   - Challenge mismatches: if the problem is minor but the appetite is 6 weeks, or vice versa, push back.

4. **Solution sketch** — *"Give me a rough idea of the solution. Not a spec — think fat-marker sketch. What's the core idea? How would the user experience change?"*
   - If the sketch is too detailed: *"That's getting into spec territory. In Shape Up, the building team figures out implementation. What's the essential concept?"*
   - If the sketch is too vague: *"I need a bit more to assess feasibility. What would the user see or do differently?"*

5. **Rabbit holes** — Actively hunt for these: *"What's the part of this that could get complicated? What assumptions are we making that might be wrong?"*
   - For each rabbit hole, ask: *"How would we cap this? What's the boundary that prevents it from spiraling?"*
   - If the user says "there are no rabbit holes": *"Really? Let me push on a few things..."* and probe technical complexity, data dependencies, integration points, edge cases.

6. **No-gos** — *"What are we deliberately NOT doing? What's tempting to include but not worth the complexity?"*
   - Help identify these: *"Would it be tempting to also do [related thing]? Should that be a no-go?"*

7. **Evidence** — Compile all evidence: customer quotes, support data, discovery briefs, competitor context, research findings. Must be specific.

### Bet type classification

Ask: *"Is this a direct user-facing improvement, or is it enabling/infrastructure work?"*

**If Tech Bet:**
> *"Tech Bets are valid, but they need a clear connection to user value. What user-facing bet does this unlock? When would we ship that user-facing bet? If we can't answer that, we should rethink whether this is shaped well enough."*

### Scoping

After the pitch outline is clear, help break it into scopes:

*"Let's break this into independent scopes — pieces of work that can each be completed and demonstrated on their own. What's the first meaningful slice a user could see?"*

For each scope:
- Must be nameable in user terms
- Must be independently demonstrable
- Must represent visible progress

If scopes need user stories: *"Want to write user stories for these scopes?"* → can invoke `/user-story` for each scope.

## Output

Produce the pitch as a structured artifact block. Use the sections from the pitch-template reference:

```
[ARTIFACT type:pitch slug:<title-slug> title:"Pitch: <Title>" date:<YYYY-MM-DD> status:Draft]
[SECTION "Problem"]
Problem in user terms with evidence. Link to discovery brief if exists.
[REF discovery:<slug>]
[SECTION "Personas affected"]
Which personas and why.
[REF persona:<slug>]
[SECTION "Appetite"]
Time budget and reasoning.
[SECTION "Solution sketch"]
Rough concept — breadboards and fat-marker sketches in words.
[SECTION "Rabbit holes"]
- Risk: description and how to cap it
[SECTION "No-gos"]
- What's excluded and why
[SECTION "Scopes"]
### Scope 1: [Name in user terms]
What this covers, why it's independent. What would the user see when done?
### Scope 2: [Name in user terms]
What this covers.
[SECTION "Evidence"]
Customer conversations, support patterns, research findings, competitive context.
[REF research:<slug>]
[REF competitor:<slug>]
[/ARTIFACT]
```

Always set status to **Draft**. The output layer writes this to the configured destination.

After writing the pitch, tell the user: *"Pitch drafted. Now let me have it reviewed."*

### Automatic review

Dispatch the **bet-reviewer agent**:

Use the Agent tool with `subagent_type: "product-craft:bet-reviewer"` and a prompt like:
> "Review the pitch titled '[title]' that was just created. Read the referenced persona and research artifacts. Check it against the review checklist in your instructions."

Present the review to the user: *"Here's the review. Let's discuss any issues and refine the pitch."*

Iterate until the user is satisfied. Then: *"The pitch is ready. When you're confident, change the status from 'Draft' to 'Ready for Betting' to put it on the table for the next cycle."*

## Contextual Handoffs

- No validated problem → *"Let's validate this problem first."* → redirects to `/discovery`
- No matching persona → *"We need a persona for this."* → offers `/personas`
- Scopes need stories → *"Want to write user stories for these scopes?"* → offers `/user-story`
- Bet challenges current strategy → *"This bet pushes against our current strategic direction [specific conflict]. Want to revisit the product strategy?"* → offers `/product-strategy`
- Competitor context needed → *"Does a competitor already solve this? Let's check."* → offers `/competitor-analysis`

## Pitch Status Lifecycle

For reference (status transitions are manual — the user edits the field):
- `/shape-bet` → **Draft**
- After bet-reviewer review + user refinement → user promotes to **Ready for Betting**
- Selected at betting table → **Bet**
- Implementation complete → **Shipped**
