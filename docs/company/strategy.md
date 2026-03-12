# Strategic Positioning

## Market Context

Stamus Networks competes in the **Network Detection and Response (NDR)** market, a segment recognized by Gartner and growing as organizations look beyond endpoint-only security.

## Competitive Landscape

| Competitor | Positioning | Stamus Contrast |
|------------|-------------|-----------------|
| **Darktrace** | AI-first, autonomous response, self-learning | Stamus: transparent detection vs. black box; explainable vs. autonomous |
| **Vectra** | AI-driven threat detection, Attack Signal Intelligence | Stamus: multi-layer detection (not AI-only); evidence-based response |
| **Corelight** | Zeek-based, network evidence, analyst-focused | Stamus: Suricata-based; real-time detection + evidence (not evidence-only) |

## Stamus Differentiators

### 1. Suricata Mastery
The founders built Suricata. No other NDR vendor has this depth of engine expertise. This translates to better rule tuning, faster detection innovation, and direct influence on the underlying technology.

### 2. Detection Transparency
Every detection is explainable. Analysts can see why an alert fired, inspect the rule or algorithm, and customize it. This contrasts with AI-only vendors where the reasoning is opaque.

### 3. Multi-Layer Detection
Not just signatures, not just AI — Stamus combines:
- Signature and IoC-based detection
- Machine learning and statistical anomaly detection
- Advanced heuristic and algorithmic detection

### 4. Declarations (DoC / DoPV)
High-fidelity "Declarations of Compromise" and "Declarations of Policy Violations" reduce alert fatigue by surfacing only high-confidence findings with supporting evidence.

### 5. Pricing Model
Flat-rate annual licensing based on monitored line speed (1G, 10G, 40G, 100G). Unlimited hosts, IPs, and users. No per-device or per-endpoint charges.

### 6. Open-Source Foundation
Clear NDR Community (SELKS successor) under GPL 3.0 provides a free on-ramp and builds community trust. Enterprise customers get the same core engine with added scale, management, and ML capabilities.

## Product-Led Growth Path

```
Clear NDR Community (free, open-source)
  → Suricata practitioners discover and adopt
  → Organizations outgrow community edition
  → Upgrade to Clear NDR Enterprise
```

## Integration Strategy

Stamus integrates broadly rather than competing with the security stack:
- **SIEMs**: Splunk (dedicated app), Google Chronicle
- **EDR/XDR**: CrowdStrike, SentinelOne, VMware Carbon Black
- **Cloud**: AWS, Google Cloud, Azure
- **SOAR**: Webhook-based workflow automation
- **Auth**: Microsoft Entra ID SAML
- **Notifications**: MS Teams, email
- **Network**: Forescout, Garland Technology, NetQuest
- **Analytics**: Elastic, OpenSearch, Sematext
- **AI**: MCP (Model Context Protocol) integration for AI-augmented analysis
