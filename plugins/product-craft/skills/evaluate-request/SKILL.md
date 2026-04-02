---
name: evaluate-request
description: Rigorously assess whether a customer request, sales ask, or internal idea deserves attention. Prevents "loudest customer wins" and forces strategic evaluation.
---

# Evaluate Request

Take a customer request, sales ask, or internal idea and rigorously assess whether it deserves attention. This skill prevents the "loudest customer wins" problem by forcing structured evaluation against personas, strategy, and evidence.

## Context Loading

Before asking anything, silently:
1. Read `product-craft.json` from the project root. If it does not exist, follow the Auto-Detection flow from the output-layer skill before proceeding.
2. Use the output layer to list all artifacts of type `persona`.
3. Use the output layer to read the artifact `strategy:product-strategy` if it exists.
4. Use the output layer to list all artifacts of type `research` and `pitch` to check for overlapping work.

## Entry Point

Ask: **"What's the request, and where did it come from?"**

Prompt for specifics: who asked (customer name/tier, sales rep, support ticket, internal team member), when, and in what context (deal negotiation, support escalation, feature request form, casual conversation).

## Evaluation Flow

Ask questions **one at a time, conversationally**.

### Required information (completeness gate)

1. **The request** — What exactly was asked for? (already from entry point)
2. **Source and context** — Who asked, their relationship with us, what's at stake? A request from a prospect in a $500K deal is different from a forum post.
3. **Solution or problem?** — Determine if the request is a solution ("add feature X") or a problem ("I can't do Y"). If it's a solution, dig for the underlying problem:
   *"That sounds like a solution. What problem is the customer trying to solve? What are they unable to do today?"*
   Keep asking until you get to the real problem. This is critical.
4. **Pattern or one-off** — "Have other customers or prospects asked for something similar? Has this come up in support tickets?"
5. **Strategic alignment** — Compare against the product strategy artifact loaded during Context Loading. Does this fit current themes?

### Overlap detection

Check if the research or pitch artifacts loaded during Context Loading already cover this:
- If a discovery brief exists → *"We already investigated this problem on [date]. Here's what we found: [summary]. Does this request add new information?"*
- If a pitch exists → *"There's already a pitch covering this: [title]. Does this request change anything about it?"*

### Scoring

Evaluate against this framework. For each dimension, rate as LOW / MEDIUM / HIGH:

1. **Strategic fit** — Does this align with our product strategy and current themes? Or is it a distraction?
2. **Persona fit** — Which defined persona benefits? How central is this to their workflow? If no persona matches, that's a signal.
3. **Evidence strength** — One customer asking once = LOW. Multiple customers across segments = HIGH. Data showing a pattern = HIGH.
4. **Effort signal** — Rough sense of effort. Not an estimate — just small (days), medium (weeks), or large (months). This helps assess proportionality.
5. **Opportunity cost** — What do we NOT do if we pursue this? Is there something more impactful we'd delay?

Present the scoring to the user: *"Here's how I'd rate this request. Do you agree?"*

### Verdict

Based on scoring, recommend one of:
- **Investigate** — Evidence is interesting but incomplete. Run a discovery session to validate.
- **Shape** — Problem is real, strategic fit is good, evidence is sufficient. Shape it into a bet.
- **Park** — Not bad, but not compelling enough right now. Revisit if more evidence emerges.
- **Decline** — Doesn't fit our strategy, affects no defined persona, or evidence is too weak. Explain why clearly so the team can communicate back to the requestor.

## Output

Produce the evaluation as a structured artifact block:

```
[ARTIFACT type:evaluation slug:<topic-slug> title:"Evaluation: <Request Summary>" date:<YYYY-MM-DD>]
[SECTION "Original request"]
What was asked for, verbatim if possible
[SECTION "Source & context"]
Who asked, when, in what context, what's at stake
[SECTION "Underlying problem (reframed)"]
The real problem behind the request — in user terms
[SECTION "Affected personas"]
Which personas match, or note if none
[REF persona:<slug>]
[SECTION "Score"]
| Dimension | Rating | Notes |
|-----------|--------|-------|
| Strategic fit | | |
| Persona fit | | |
| Evidence strength | | |
| Effort signal | | |
| Opportunity cost | | |
[SECTION "Verdict"]
Investigate / Shape / Park / Decline
[SECTION "Reasoning"]
Why this verdict, what would change it
[/ARTIFACT]
```

The output layer writes this to the configured destination. For local files, this renders to `{basePath}/research/eval-{date}-{slug}.md`.

## Contextual Handoffs

- Verdict "Investigate" → *"Want to run a discovery session to dig deeper into this problem?"*
- Verdict "Shape" → *"This looks ready to shape. Want to start writing a bet pitch?"*
- If request mentions a competitor feature with no profile → *"The request references [competitor feature]. We don't have a profile for [competitor]. Want to run a competitor analysis?"*
- If no persona matches the requester → *"This request doesn't map to any of our defined personas. Either we're missing a persona, or this user isn't in our target market. Which is it?"*
