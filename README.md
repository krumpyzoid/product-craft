# Product Craft

A Claude Code plugin and shared workspace for Product Management. Built around Shape Up methodology, it provides AI-assisted skills for discovery, competitor analysis, bet shaping, user stories, and product strategy.

## Why this exists

Engineering-led teams tend to jump to solutions before understanding problems, frame work in technical terms instead of user impact, and lack a formal PM function. This plugin fills that gap.

## Installation

Install the plugin from GitHub into any project:

```
/plugin marketplace add krumpyzoid/product-craft
/plugin install product-craft@product-craft
```

That's it. The plugin's skills and commands are now available in your project.

### Alternative: clone as a workspace

If you want a standalone product workspace (with the `docs/` directory for artifacts):

```bash
git clone https://github.com/krumpyzoid/product-craft.git
cd product-craft
```

Open it in [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and the plugin loads automatically.

## Getting started

Run `/product` to see the hub, or use any command directly.

## Commands

| Command | What it does |
|---------|-------------|
| `/product` | Hub — shows status of all artifacts and routes you to the right skill |
| `/personas` | Create and maintain user personas grounded in real data |
| `/competitor-analysis` | Build deep competitor profiles |
| `/market-research` | Research industry trends and market dynamics |
| `/discovery` | Validate whether a problem is real, worth solving, and for whom |
| `/evaluate-request` | Assess whether a customer request or idea deserves attention |
| `/user-story` | Write user stories in user terms, not technical terms |
| `/shape-bet` | Shape a validated problem into a Shape Up pitch |
| `/product-strategy` | Big picture — positioning, themes, and cycle planning |

## How it works

**Skills ask questions first.** Every skill has a completeness gate — it won't produce output until it has enough information. It asks clarifying questions one at a time and validates your answers against web research and existing artifacts.

**Problem before solution.** If you say "we need feature X", the skill will redirect: "What problem would that solve? Who's hitting it?" This is by design.

**Everything connects.** If a new persona comes up during competitor analysis, the skill offers to create it. If a discovery session validates a problem, it offers to shape a bet. Skills are aware of each other through shared artifacts in `docs/`.

**Artifacts are markdown.** All output lives in `docs/` and is versioned with git. Review, discuss, and iterate through normal git workflows.

## Artifacts

```
docs/
├── company/          # Values, brand, company strategy
├── personas/         # User persona profiles
├── competitors/      # Competitor profiles and comparisons
├── pitches/          # Shape Up pitches (bets)
│   └── stories/      # Standalone user stories
├── research/         # Market research, discovery briefs, evaluations
└── strategy/         # Product strategy
```

## Shape Up workflow

This plugin supports the [Shape Up](https://basecamp.com/shapeup) methodology:

1. **Discover** — Use `/discovery` to validate problems before jumping to solutions
2. **Shape** — Use `/shape-bet` to write pitches with appetite, scopes, rabbit holes, and no-gos
3. **Bet** — Pitches start as Draft, get reviewed by the bet-reviewer agent, then promoted to Ready for Betting
4. **Build** — Selected bets become active work for the cycle

Pitches follow a status lifecycle: **Draft** → **Ready for Betting** → **Bet** → **Shipped**

## Agents

The plugin includes two specialized agents:

- **market-researcher** — Dispatched by skills to do web research (competitor sites, analyst reports, industry trends). Returns structured findings with sources.
- **bet-reviewer** — Reviews shaped pitches for common pitfalls: too technical, missing user perspective, weak evidence, unclear scopes.

## Contributing

Use the skills to generate artifacts, commit them, and push. The repo is the shared source of truth for product decisions.

When adding or modifying skills, the plugin source lives in `plugins/product-craft/`.
