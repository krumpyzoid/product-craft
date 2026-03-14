# Product Craft Plugin — Design Spec

## Overview

A Claude Code plugin called **product-craft** that provides Product Management skills for engineering-led teams. It fills the gap of missing PM culture by forcing problem-first, user-centric thinking across competitor analysis, market research, discovery, request evaluation, user stories, Shape Up bet shaping, product strategy, and persona management.

The plugin targets an engineering-led team adopting Shape Up, where bets tend to be too technical, user needs are underexplored, and the team jumps to solutions before identifying problems.

## Architecture

### Single plugin, many skills

One plugin with individual skills, a hub command, and two specialized agents. Skills are invoked standalone or via the hub. All skills share context through artifacts stored in the project repo.

### Plugin structure

```
product-craft/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── product.md
│   ├── competitor-analysis.md
│   ├── market-research.md
│   ├── discovery.md
│   ├── evaluate-request.md
│   ├── user-story.md
│   ├── shape-bet.md
│   ├── product-strategy.md
│   └── personas.md
├── skills/
│   ├── competitor-analysis/SKILL.md
│   ├── market-research/SKILL.md
│   ├── discovery/SKILL.md
│   ├── evaluate-request/SKILL.md
│   ├── user-story/SKILL.md
│   ├── shape-bet/SKILL.md
│   ├── product-strategy/SKILL.md
│   └── personas/SKILL.md
├── agents/
│   ├── market-researcher.md
│   └── bet-reviewer.md
└── references/
    ├── shape-up-primer.md
    └── pitch-template.md
```

### Plugin manifest

`.claude-plugin/plugin.json`:
```json
{
  "name": "product-craft",
  "description": "Product Management skills for Shape Up teams: discovery, competitor analysis, bet shaping, user stories, and product strategy",
  "version": "0.1.0",
  "author": {
    "name": "Product Craft"
  }
}
```

### Command file format

Each command file under `commands/` is a thin routing stub that invokes the corresponding skill. Commands contain no logic — they are routing layers. Example (`commands/competitor-analysis.md`):
```markdown
---
description: "Build and maintain competitor profiles with web research and internal knowledge"
disable-model-invocation: true
---

Invoke the product-craft:competitor-analysis skill and follow it exactly as presented to you.
```

### Agent invocation

Skills instruct the main agent to dispatch subagents using the Agent tool. When a skill says "dispatch the market-researcher agent", it means the SKILL.md prompt should instruct the main agent to use the Agent tool with `subagent_type: "product-craft:market-researcher"` and a focused research brief as the prompt. The agent runs in the background or foreground depending on whether the skill needs results before continuing.

If web tools (WebFetch, WebSearch) are unavailable, agents degrade gracefully by asking the user to provide the information manually.

### Artifact directory (in project repo)

```
docs/
├── company/
│   ├── core-values.md
│   ├── brand.md
│   └── strategy.md
├── personas/
├── competitors/
├── pitches/
│   └── stories/
├── research/
└── strategy/
```

The plugin contains the process (skills, agents). The artifacts live in the project repo under `docs/` so they're versioned, reviewable, and accessible to the whole team.

## Core Design Principles

### 1. Completeness gates

No skill produces output until it has enough signal. Each skill defines required inputs and asks clarifying questions conversationally, focusing on one topic at a time rather than presenting a checklist. The skill explicitly tells the user what's still missing.

### 2. Problem-first enforcement

Skills actively push back on solution-thinking. When a user says "we need to add feature X", the skill reframes: "What problem would that feature solve? Who's hitting that problem?" This applies across all skills, especially `/discovery`, `/evaluate-request`, `/shape-bet`, and `/user-story`.

### 3. Contextual handoffs

Skills detect when a new persona, competitor, or insight emerges during their workflow and offer to branch into the relevant skill:

- New competitor mentioned with no profile → offers `/competitor-analysis`
- New user type doesn't match existing personas → offers `/personas`
- Bet targets a market segment with no research → offers `/market-research`
- Request mentions a competitor feature → offers to pull up their profile

Each skill checks `docs/` at the start for existing artifacts. When it encounters a reference to something that doesn't exist, it offers the handoff. The user can accept or decline.

### 6. Staleness and conflict awareness

When loading existing artifacts, skills check the `Last updated` date. If an artifact is older than 3 months, the skill flags it: "The Darktrace profile was last updated 6 months ago — want to refresh it before we continue?" When artifacts conflict (e.g., strategy says "we're not doing X" but a pitch is being shaped for X), the skill surfaces the conflict and asks the user to resolve it before proceeding.

### 4. Web research + human validation

Skills do what they can with web research (via the market-researcher agent), then prompt the user to fill gaps with internal knowledge — what sales is hearing, what support tickets say, what customers said in calls. If web tools are unavailable, skills degrade gracefully by asking the user to provide the information manually.

### 5. User-perspective framing

All artifacts are written from the user's perspective, not technical implementation. Scopes in bets are expressed as user-visible progress. User stories describe goals, not endpoints. This is enforced by the skills and validated by the bet-reviewer agent.

## Hub Command: `/product`

Entry point that routes to individual skills conversationally.

**Behavior:**
1. Reads `docs/` to surface context: existing personas, competitor profiles, pitches, research
2. Presents a brief status summary: "You have 3 personas, 2 competitor profiles, 1 pitch in draft..."
3. Asks conversationally: "What are you working on?" and based on the response, routes to the most relevant skill:
   - Understand the market → invokes `/competitor-analysis` or `/market-research`
   - Understand users → invokes `/discovery` or `/personas`
   - Evaluate an idea or request → invokes `/evaluate-request`
   - Shape work → invokes `/shape-bet` or `/user-story`
   - Think about direction → invokes `/product-strategy`
4. If the intent is ambiguous, asks a follow-up question to narrow down

Each command also works standalone — `/shape-bet` can be invoked directly without going through the hub.

## Skills

### `/personas`

**Purpose:** Create, maintain, and reference user personas grounded in real data.

**Completeness gate — needs before producing:**
- Role/title and organization type
- Daily responsibilities and workflows
- Tools they use (SIEM, SOAR, ticketing, etc.)
- Pain points and frustrations
- Goals and what success looks like for them
- How they interact with Clear NDR specifically
- Technical skill level

**Process:**
1. Checks `docs/personas/` for existing personas
2. Asks: create new, update existing, or list all?
3. If creating: asks clarifying questions one at a time, does web research on the role, validates with user's internal knowledge
4. Produces a structured persona file in `docs/personas/<role-slug>.md`
5. If updating: loads the existing persona, asks what changed, updates

**Persona file format:**
```markdown
# [Role Name]
## Who they are
## Their day-to-day
## Pain points
## Goals
## Relationship with Clear NDR
## Quotes (real or synthesized from research)
## Last updated: YYYY-MM-DD
```

**Contextual handoffs:**
- If user describes a persona that overlaps with an existing one → flags it
- Other skills reference personas by linking to these files

---

### `/competitor-analysis`

**Purpose:** Build and maintain deep competitor profiles. Understand positioning, strengths, weaknesses, and how Clear NDR compares.

**Completeness gate — needs before producing:**
- Which competitor (or discover new ones)
- What aspect to focus on (full profile, specific feature comparison, pricing, positioning, recent moves)
- How Clear NDR currently competes against them on this aspect
- What your sales team hears in deals against this competitor

**Process:**
1. Checks `docs/competitors/` for existing profiles
2. Asks: new competitor, update existing, or compare two competitors?
3. Dispatches market-researcher agent to fetch competitor website, product pages, recent blog posts, analyst mentions
4. Asks user to fill gaps: "What do your sales reps say when they lose a deal to Darktrace?"
5. Produces structured profile in `docs/competitors/<competitor-slug>.md`
6. If comparison requested, produces side-by-side in `docs/competitors/comparison-<a>-vs-<b>.md`

**Competitor profile format:**
```markdown
# [Competitor Name]
## Overview
## Product & Technology
## Target market
## Pricing model
## Strengths (from their perspective)
## Weaknesses (from our perspective + market evidence)
## How they position against us
## How we win against them
## How we lose against them
## Recent moves (acquisitions, launches, partnerships)
## Last updated: YYYY-MM-DD
```

**Contextual handoffs:**
- Competitor feature maps to unmet need → offers `/discovery`
- Comparison reveals a gap that could be a bet → offers `/shape-bet`

---

### `/market-research`

**Purpose:** Understand industry trends, market dynamics, and where the NDR/network security space is heading.

**Completeness gate — needs before producing:**
- Research question or area of focus
- Scope: broad landscape or deep dive on specific topic
- What you already know or suspect (to validate or challenge)

**Process:**
1. Checks `docs/research/` for existing research that might overlap
2. Asks what triggered this research
3. Dispatches market-researcher agent to search analyst reports, industry publications, vendor announcements, community discussions
4. Synthesizes findings, validates with user
5. Calls out where data is thin or conflicting
6. Produces research brief in `docs/research/YYYY-MM-DD-<topic>.md`

**Research brief format:**
```markdown
# [Topic]
## Research question
## Key findings
## Evidence (with sources)
## What this means for Clear NDR
## Open questions (what we still don't know)
## Recommended next steps
## Last updated: YYYY-MM-DD
```

**Contextual handoffs:**
- Findings reveal unserved persona → offers `/personas`
- Findings suggest strategic opportunity → offers `/product-strategy`
- Specific competitor keeps appearing → offers `/competitor-analysis`

---

### `/discovery`

**Purpose:** Validate whether a problem is real, worth solving, and for whom. Prevents jumping to solutions.

**Completeness gate — needs before producing:**
- What triggered this investigation (customer complaint, sales feedback, internal hunch, competitor feature)
- Who has this problem (maps to a persona or identifies a new one)
- How they cope today (workaround, competitor tool, manual process, they just suffer)
- How often they hit this problem and how painful it is
- Evidence: one loud customer or a pattern?
- What happens if we don't solve it

**Process:**
1. Checks `docs/personas/` and `docs/research/` for existing context
2. Starts with: "What made you think about this?"
3. Actively pushes back on solution-thinking. If user says "we need to add X feature", reframes: "What problem would that feature solve? Who's hitting that problem?"
4. Does web research to check if this is industry-wide or unique
5. Scores the problem:
   - Frequency: How often does this happen?
   - Severity: How bad is it when it does?
   - Breadth: How many personas/segments are affected?
   - Trend: Is this getting worse?
6. Produces discovery brief in `docs/research/discovery-YYYY-MM-DD-<topic>.md`

**Discovery brief format:**
```markdown
# Discovery: [Problem Statement]
## Trigger
## Affected personas
## Problem description (user's perspective, not technical)
## Current workarounds
## Evidence
## Problem score
| Dimension | Rating | Notes |
|-----------|--------|-------|
| Frequency | | |
| Severity  | | |
| Breadth   | | |
| Trend     | | |
## What happens if we don't solve this
## Recommended next step (shape a bet? more research? park it?)
## Last updated: YYYY-MM-DD
```

**Contextual handoffs:**
- Problem validated and worth solving → offers `/shape-bet`
- Affected user type is new → offers `/personas`
- Workaround involves a competitor → offers `/competitor-analysis`

**Key design choice:** This skill is deliberately confrontational. It challenges assumptions and asks for evidence.

---

### `/evaluate-request`

**Purpose:** Rigorously assess whether a customer request, sales ask, or internal idea deserves attention. Prevents "loudest customer wins."

**Completeness gate — needs before producing:**
- The request itself (what was asked for, by whom)
- Source and context (customer size/tier, deal at stake, support ticket, internal idea)
- Is this a solution or a problem? (if solution, dig for underlying problem)
- How many others have asked for something similar
- Strategic alignment

**Process:**
1. Loads existing personas from `docs/personas/` and strategy from `docs/strategy/`
2. Asks: "What's the request, and where did it come from?"
3. If request is a solution, reframes to find the underlying problem
4. Checks for existing discovery briefs or bets that cover this
5. Evaluates against scoring framework:
   - Strategic fit: Does this align with product strategy?
   - Persona fit: Which persona benefits? How many?
   - Evidence strength: One customer or a pattern?
   - Effort signal: Small, medium, large? (rough, not an estimate)
   - Opportunity cost: What don't we do if we do this?
6. Produces evaluation in `docs/research/eval-YYYY-MM-DD-<topic>.md`

**Evaluation format:**
```markdown
# Evaluation: [Request Summary]
## Original request
## Source & context
## Underlying problem (reframed)
## Affected personas
## Score
| Dimension | Rating | Notes |
|-----------|--------|-------|
| Strategic fit | | |
| Persona fit | | |
| Evidence strength | | |
| Effort signal | | |
| Opportunity cost | | |
## Verdict: Investigate / Shape / Park / Decline
## Reasoning
## Last updated: YYYY-MM-DD
```

**Contextual handoffs:**
- Verdict "Investigate" → offers `/discovery`
- Verdict "Shape" → offers `/shape-bet`
- Request mentions unknown competitor feature → offers `/competitor-analysis`

---

### `/user-story`

**Purpose:** Write user stories that express need and impact from the user's perspective, not technical implementation.

**Completeness gate — needs before producing:**
- Which persona this is for (must exist in `docs/personas/` or create one)
- What problem this addresses (should link to discovery brief or evaluation)
- What the user is trying to accomplish (goal, not feature)
- How they'll know it worked (acceptance from user's perspective)
- What they do today without this

**Process:**
1. Loads personas from `docs/personas/` and relevant discovery/evaluation briefs
2. Asks: "Which persona is this for?" — presents existing personas
3. If user jumps to technical language, pushes back: "That's implementation. What is the person trying to accomplish?"
4. Builds the story iteratively through questions
5. Produces stories either embedded in a pitch (when invoked from `/shape-bet`) or standalone in `docs/pitches/stories/YYYY-MM-DD-<title>.md`

**Story format:**
```markdown
## User Story: [Short title]
**Persona:** [Link to persona]
**Problem:** [Link to discovery/evaluation if exists]

As a [persona], I want to [goal in their language]
so that [impact on their work/life].

### Acceptance criteria
- [ ] [Observable outcome, not technical implementation]
- [ ] [Observable outcome]

### What they do today
[Current workaround or pain]

### What changes for them
[The "after" — how their experience improves]
```

**Key design choice:** This skill refuses to write stories in technical terms. It's relentless about user-perspective framing.

**Contextual handoffs:**
- No matching persona → offers `/personas`
- No validated problem → offers `/discovery`
- Stories ready and grouped → offers `/shape-bet`

---

### `/shape-bet`

**Purpose:** The core Shape Up artifact. Takes a validated problem and shapes it into a pitch with clear boundaries, appetite, and risks.

**Completeness gate — needs before producing a pitch:**
- Problem statement (validated — links to discovery brief or strong evidence)
- Who has this problem (persona reference)
- Appetite: how much time are we willing to spend? (not "how long will it take" but "how much is this worth")
- Solution sketch (rough — the "what" not the "how")
- Rabbit holes: what could go wrong or spiral
- No-gos: what are we explicitly NOT doing
- Evidence that this is worth betting on

**Process:**
1. Loads existing artifacts: personas, discovery briefs, evaluations, competitor profiles, strategy
2. Asks: "What problem are we shaping a bet around?"
3. If user starts with a solution, redirects to find the problem — may hand off to `/discovery`
4. Walks through appetite: "If this problem disappeared tomorrow, how valuable would that be? Is this a 1-week small batch or a 6-week big batch?"
5. Actively hunts for rabbit holes: "What's the part that could get complicated? What assumptions are we making?"
6. Asks about no-gos: "What are we deliberately cutting?"
7. Distinguishes between Bets (user-facing value) and Tech Bets (infrastructure/enablement). For Tech Bets, still requires: "What user-facing bet does this unlock? When?"
8. Produces a pitch in `docs/pitches/YYYY-MM-DD-<title>.md`
9. Dispatches bet-reviewer agent to review the draft pitch against the review checklist, then presents the review to the user for discussion and refinement

**Pitch format:**
```markdown
# Pitch: [Title]
## Status: Draft | Ready for Betting | Bet | Shipped

## Problem
[In user terms, with evidence. Links to discovery brief.]

## Personas affected
[Links to persona files]

## Appetite
[Time budget and why this amount is right]

## Solution sketch
[Rough concept — enough to show it's feasible, not a spec.
Breadboards and fat-marker sketches in words.]

## Rabbit holes
- [Risk 1: what could spiral, and how we'd cap it]
- [Risk 2]

## No-gos
- [Explicitly out of scope]

## Scopes
[Break the work into independent pieces that can be completed
and demonstrated individually.]

### Scope 1: [Name]
[What this covers, why it's independent]

### Scope 2: [Name]
[What this covers]

## Tech Bet?
[If yes: what user-facing bet does this unlock?]

## Evidence
[Customer conversations, data, competitor analysis, discovery briefs]

## Last updated: YYYY-MM-DD
```

**Pitch status lifecycle:**
- `/shape-bet` always produces pitches with status **Draft**
- After bet-reviewer agent review and user refinement, user promotes to **Ready for Betting**
- At the betting table (a human ceremony — the plugin supports it by listing all "Ready for Betting" pitches via `/product`), selected pitches become **Bet**
- After implementation completes, status moves to **Shipped**

Status transitions are manual (the user edits the status field). The plugin does not auto-promote.

**Key design choice:** Scopes are expressed as user-visible progress, not technical tasks. The skill refuses to produce a pitch without evidence.

**Contextual handoffs:**
- No validated problem → redirects to `/discovery`
- No persona → offers `/personas`
- Scopes need user stories → offers `/user-story`
- Bet challenges a strategic assumption → offers `/product-strategy`

---

### `/product-strategy`

**Purpose:** Step back from individual bets and think about the big picture. Where is Clear NDR heading, how to position against competitors, what themes for the next cycle.

**Completeness gate — needs before producing:**
- Time horizon (next cycle? 6 months? 1 year?)
- What's changed since last strategy review
- Current strengths to double down on
- Current weaknesses to address or accept
- What bets and pitches already exist

**Process:**
1. Loads everything: `docs/strategy/`, `docs/competitors/`, `docs/personas/`, `docs/pitches/`, `docs/research/`
2. Starts with: "What's prompting this? New cycle starting? Something shifted?"
3. Walks through strategic framework:
   - Where are we now? — Current positioning, strengths, what customers love
   - Where do we want to be? — Vision for Clear NDR in [time horizon]
   - What's in our way? — Gaps, competitor pressure, resource constraints
   - What themes emerge? — Groups existing pitches, research, and competitor intel
4. Does web research on market direction if needed
5. Helps prioritize: "Given your small team, you can realistically pursue 2-3 themes. Which matter most?"
6. Produces/updates `docs/strategy/product-strategy.md`

**Strategy format:**
```markdown
# Product Strategy
## Last updated: YYYY-MM-DD
## Time horizon: [period]

## Positioning
[How Clear NDR wins — our unique angle]

## Strategic themes
### Theme 1: [Name]
- Why this matters
- Supporting evidence (links to research, competitors, pitches)
- Key bets in this theme

### Theme 2: [Name]
...

## What we're NOT doing (and why)
- [Deliberate trade-off]

## Key risks
- [Strategic risk and how we're watching it]

## Inputs
[Links to competitor profiles, research briefs, and pitches that informed this]
```

**Contextual handoffs:**
- Theme has no supporting research → offers `/market-research`
- Theme implies a new persona → offers `/personas`
- Theme needs shaping into a bet → offers `/shape-bet`
- Competitive positioning is stale → offers `/competitor-analysis`

## Agents

### market-researcher

**Purpose:** Subagent dispatched by skills to do web research without cluttering the main conversation.

**Model:** sonnet
**Color:** green
**Tools:** Glob, Grep, Read, WebFetch, WebSearch

**Behavior:**
- Receives a focused research brief from the parent skill
- Searches multiple sources, cross-references claims
- Returns structured findings with sources cited
- Flags uncertain, outdated, or conflicting information
- Does NOT make strategic recommendations — that's for the parent skill in conversation with the user

---

### bet-reviewer

**Purpose:** Reviews a shaped pitch for common pitfalls. Acts as a product coach catching patterns: too technical, missing user perspective, no evidence.

**Model:** sonnet
**Color:** yellow
**Tools:** Glob, Grep, Read

**Review checklist:**
- Is the problem stated in user terms, not technical terms?
- Is there a real persona attached, not a generic "user"?
- Is there evidence beyond "we think this is important"?
- Are scopes expressed as user-visible progress?
- Are rabbit holes identified or is the bet naively optimistic?
- Are no-gos explicit?
- If it's a Tech Bet, is the user-facing unlock clear?
- Does the appetite match the problem's importance?

Returns structured review: what's strong, what's missing, specific suggestions.

## References

### shape-up-primer.md

A concise reference on Shape Up concepts so the skills stay consistent with the methodology. Covers:
- **Shaping vs. building** — shaping is a senior/strategic activity, building is for the team
- **Appetite** — a time budget set upfront, not an estimate; "how much is this worth?" not "how long will it take?"
- **Bets** — shaped pitches selected at the betting table for a cycle
- **Scopes** — independent slices of work within a bet that can be completed and demonstrated individually
- **Rabbit holes** — identified risks that could blow up the appetite; must be addressed in the pitch
- **No-gos** — explicitly excluded functionality to constrain scope
- **Cool-down** — unscheduled time between cycles for cleanup, exploration, and recovery
- **Betting table** — the ceremony where pitches are selected for the next cycle; not a backlog grooming — unselected pitches are not queued, they're reconsidered fresh next time
- **Hill charts** — visual progress tracking showing work moving from "figuring it out" to "making it happen"

### pitch-template.md

The canonical pitch structure used by `/shape-bet`. This is the single source of truth for the pitch format — the inline format shown in the `/shape-bet` skill definition mirrors this template. Contains the structure with placeholder text and guidance comments explaining what each section should contain.

## Future: GitLab MCP Integration

When GitLab MCP access is available, skills can:
- Create epics from pitches
- Create issues from user stories and scopes
- Sync pitch status with milestones
- Pull customer request data from issues

This is out of scope for the initial implementation. The markdown-first approach ensures all artifacts are useful immediately and GitLab sync is additive.
