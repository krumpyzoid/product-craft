# Pitch Template

This is the canonical pitch format for product-craft. The `/shape-bet` skill uses this template when producing pitches. This is the single source of truth for pitch structure.

---

```
# Pitch: [Title — short, descriptive, in user terms]
## Status: Draft

<!-- Status lifecycle:
  Draft → Ready for Betting → Bet → Shipped
  - /shape-bet always produces Draft
  - After bet-reviewer review + user refinement → user promotes to Ready for Betting
  - Selected at betting table → Bet
  - Implementation complete → Shipped
  All transitions are manual. -->

## Problem
<!-- Describe the problem in user terms, not technical terms.
  Who has this problem? How do they experience it?
  Link to discovery brief if one exists.
  Include evidence: customer quotes, support tickets, data, competitive pressure. -->

## Personas affected
<!-- Reference personas using [REF persona:<slug>] notation.
  Be specific — which personas and why. -->

## Appetite
<!-- Time budget: 1-week small batch, 2-week small batch, or 6-week big batch.
  Explain why this amount is right.
  Remember: appetite is "how much is this worth?" not "how long will it take?" -->

## Solution sketch
<!-- Rough concept — enough to show feasibility, NOT a specification.
  Think breadboards and fat-marker sketches in words.
  What's the core idea? How would the user experience change?
  Leave room for the building team to figure out implementation. -->

## Rabbit holes
<!-- Risks that could blow up the appetite.
  For each: what could spiral, and how we'd cap it.
  A pitch without rabbit holes is naively optimistic. -->
- [Risk: description and how to cap it]

## No-gos
<!-- Explicitly out of scope. What we are NOT doing.
  "We will NOT do X" is as important as "we will do Y." -->
- [What's excluded and why]

## Scopes
<!-- Break work into independent, demonstrable pieces.
  Each scope should be completable and visible to users independently.
  Name scopes in user terms, not technical terms.
  These are starting points — the building team may discover new scopes. -->

### Scope 1: [Name in user terms]
<!-- What this covers, why it's independent.
  What would the user see when this scope is done? -->

### Scope 2: [Name in user terms]
<!-- What this covers. -->

## Tech Bet?
<!-- If this is a tech bet (infrastructure/enablement):
  What user-facing bet does this unlock? When?
  If you can't answer this, reconsider whether this bet is shaped well enough.
  If this is NOT a tech bet, remove this section. -->

## Evidence
<!-- What makes us believe this is worth betting on?
  Customer conversations, support ticket patterns, usage data,
  competitor analysis, discovery briefs.
  Reference relevant artifacts using [REF research:<slug>] and [REF competitor:<slug>] notation. -->

## Last updated: YYYY-MM-DD
```
