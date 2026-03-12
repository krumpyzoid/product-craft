# Shape Up — Quick Reference

This document is a reference for product-craft skills. It defines Shape Up terminology and principles so that all skills use consistent language and enforce correct methodology.

## Core Principle: Shaping vs. Building

Shaping is a senior/strategic activity done by a small group before work is committed. Building is what the team does during a cycle. Shaping produces a pitch — not a backlog item, not a spec, not a list of tasks.

A pitch defines the problem, a rough solution direction, boundaries (appetite, rabbit holes, no-gos), and evidence. It does NOT define implementation details, task breakdowns, or technical specs. That's the building team's job.

## Appetite

Appetite is a time budget set upfront. It answers "how much is this worth to us?" — NOT "how long will it take?"

- Small batch: 1-2 weeks
- Big batch: 6 weeks
- If it can't fit in 6 weeks, it needs to be broken down or the scope needs to be cut

Appetite is a constraint, not an estimate. The team must find a solution that fits within the appetite, or the bet is abandoned — not extended.

## Bets

A bet is a shaped pitch that has been selected at the betting table for the next cycle. Key properties:

- **Bets have a circuit breaker.** If the work isn't done by the end of the cycle, it doesn't automatically continue. It goes back to shaping.
- **Bets are commitments.** Once bet on, the team gets uninterrupted time to work on it.
- **There is no backlog.** Unselected pitches are not queued. They can be re-pitched in a future cycle if still relevant.

## Scopes

Scopes are independent slices of work within a bet. Each scope:

- Can be completed and demonstrated independently
- Represents meaningful progress visible to users
- Is named in user terms, not technical terms (e.g., "Analysts can filter by threat severity" not "Implement severity filter API")

Scopes are discovered during building, not prescribed during shaping. The pitch provides rough scopes as a starting point.

## Rabbit Holes

Rabbit holes are parts of the solution that could spiral in complexity and blow up the appetite. During shaping, you identify them and either:

- Define how to cap them (e.g., "We'll support CSV export only, not arbitrary formats")
- Cut them entirely (move to no-gos)

A pitch without identified rabbit holes is naively optimistic.

## No-Gos

No-gos are things explicitly excluded from the bet. They prevent scope creep by making the boundaries visible. "We will NOT do X" is as important as "we will do Y."

## Cool-Down

Cool-down is unscheduled time between cycles. The team uses it for:

- Bug fixes and cleanup
- Exploration and prototyping
- Personal projects and learning
- Recovery from the previous cycle

Cool-down is not optional — it prevents burnout and creates space for serendipity.

## Betting Table

The betting table is a meeting where stakeholders review shaped pitches and decide what to bet on for the next cycle. Key principles:

- Only shaped pitches (status: "Ready for Betting") are considered
- Pitches are evaluated on their own merit, not relative priority in a backlog
- Unselected pitches are NOT queued — they must be re-pitched if still relevant
- The output is a small number of bets that fill the cycle

## Hill Charts

Hill charts track progress visually. Work moves through two phases:

- **Uphill (figuring it out):** Uncertainty, exploration, unknowns
- **Downhill (making it happen):** Execution, known work, convergence

Scopes that are stuck on the uphill side signal risk. Scopes that move quickly downhill signal confidence.

## Tech Bets

Tech bets are bets on infrastructure, architecture, or enablement work. They follow the same shaping process, with one additional requirement: **every tech bet must identify what user-facing bet it unlocks and when.**

"Refactor the detection engine" is not a valid tech bet. "Refactor the detection engine so we can ship behavioral analysis for encrypted traffic next cycle" is.
