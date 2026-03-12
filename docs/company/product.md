# Product Overview — Clear NDR

## Product Suite

### Clear NDR Enterprise (formerly Stamus Security Platform)
The flagship commercial NDR platform for enterprise-scale network threat detection and response.

### Clear NDR Community (formerly SELKS)
Open-source (GPL 3.0) turnkey Suricata implementation for smaller deployments, researchers, and education. Built on Suricata 8.0.

## Architecture

Clear NDR uses a two-component architecture:

### Clear NDR Probes
Network sensors performing:
- Deep packet inspection (DPI)
- Real-time threat detection
- Metadata enrichment
- Protocol transaction capture
- Flow data collection
- File extraction
- Full packet capture (PCAP)

### Clear NDR Central Server
Centralized management and analysis:
- Probe management and orchestration
- Third-party threat intelligence integration
- Consolidated event storage
- Machine learning and algorithmic threat detection
- Automated event triage with tagging and classification
- Threat hunting and incident investigation interface

## Detection Methods

| Method | Description |
|--------|-------------|
| **Signature / IoC** | Rule-based detection using Suricata signatures and indicators of compromise |
| **AI / ML** | Statistical anomaly detection and machine learning models |
| **Heuristic / Algorithmic** | Advanced behavioral and algorithmic detections |
| **Threat Intelligence** | Stamus Threat Intelligence (STI) with daily updates |

## Key Capabilities

### Declarations of Compromise (DoC)
High-fidelity alerts that correlate multiple signals to declare with high confidence that a host is compromised. Designed to eliminate alert fatigue.

### Declarations of Policy Violations (DoPV)
Detections of policy violations — unauthorized applications, protocols, or behaviors on the network.

### Host Insights
Host-centric monitoring that aggregates network activity per host, providing analysts with a unified view of what each host is doing on the network.

### Outlier Alerts
Statistical anomaly detection that identifies unusual patterns in network behavior without requiring pre-written rules.

### Conditional Logging
Selective, event-driven data collection that captures detailed logs only when specific conditions are met — reducing storage requirements while retaining forensic value.

### Threat Hunting
Structured threat hunting workflows with predefined filters and guided investigation paths.

### Security Posture
Assessment and reporting on the organization's security posture based on network observations.

## Deployment Options

- **On-premise** — physical appliances or virtual machines
- **Private cloud** — VMware, KVM, etc.
- **Public cloud** — AWS, Google Cloud, Azure
- **Hybrid** — mixed on-prem and cloud
- **Air-gapped** — fully offline deployments for data sovereignty
- **Multi-tenant** — MSSP and multi-organization deployments

## Licensing Model

- Annual software license
- Based on **monitored link line rate** (1G, 10G, 40G, 100G increments)
- **Flat-rate** — unlimited hosts, IPs, and users
- Single license covers both Probe and Central Server
- Includes: daily threat intelligence updates, weekly threat reports, technical support, quarterly software updates, API access

## Clear NDR Community Components

| Component | Role |
|-----------|------|
| **Suricata** | Network analysis and threat detection engine |
| **Fluentd** | Data collection and forwarding |
| **OpenSearch** | Search and observability |
| **EveBox** | Alert and event management |
| **Arkime** | Network analysis and packet capture |
| **Scirius** | Suricata ruleset management (Stamus-developed) |
| **StamusCtl** | Single Go binary orchestrating all components |

### Community Deployment Methods
1. **Docker** — containerized, minutes to deploy on any Linux system
2. **ISO with Desktop** — Debian 12 with graphical interface
3. **ISO Headless** — Debian 12 server installation

## Integrations

### Security Platforms
- CrowdStrike (host containment)
- SentinelOne (Singularity Endpoint)
- VMware Carbon Black
- Proofpoint

### SIEM / Analytics
- Splunk (dedicated Stamus App)
- Google Chronicle
- Elastic / OpenSearch
- Sematext

### Cloud
- AWS, Google Cloud, Microsoft Azure

### Workflow / Automation
- Webhooks (conditional, event-triggered)
- REST API + Python SDK
- SOAR integration examples
- Deep linking to GUI views

### Identity / Notifications
- Microsoft Entra ID (SAML)
- Microsoft Teams
- Conditional email alerts

### Network
- Forescout eyeExtend
- Garland Technology
- NetQuest
- NEOX Networks

### AI
- MCP (Model Context Protocol) integration

## Open-Source Contributions

- Suricata Language Server
- GopherCAP (PCAP tool)
- Splunk App for Suricata
- Scirius (ruleset management)
- StamusCtl
- SELKS / Clear NDR Community
