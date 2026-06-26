# grc-tools

Open-source GRC (Governance, Risk, and Compliance) policy templates, tools, and guidance - built from real-world deployment experience and aligned with industry frameworks.

## What's Here

### Policy-Templates/

Production-ready policy templates in a consistent ISP (Information Security Program) format. Each policy lives in its own folder with:

- **Template (.md):** Fill-in-the-blanks policy document using industry-standard structure - defines WHAT is required.
- **Procedures (.md):** Companion implementation procedures with multiple approaches and pitfalls - defines HOW to implement the policy.
- **README:** Plain-English explanation of the policy, common gotchas, and implementation advice, plus a document-structure section explaining the policy/procedure split.

Policies and procedures are deliberately separated: policies are written for a broad audience (employees, auditors, executives); procedures are written for implementers (IT, security, HR teams). This follows the industry consensus that mixing the two creates version-control headaches, audience confusion, and audit friction.

These templates are framework-aligned (NIST 800-53, SOC 2, ISO 27001) and reflect operational lessons learned from deploying security programs in real organizations - not theoretical best practices from a textbook.

### Design Principles

- **Actually useful.** Templates include implementation advice you won't find in ISO standards - things like "logging gets expensive fast, here's what you actually need to log."
- **Industry standard format.** ISP house format (Purpose → Scope → Policy → Roles → Enforcement → Related Documents → Revision History).
- **Fill-in-the-blanks.** Underscores (`____`) mark where you need to customize - your company name, your tools, your people.
- **Tooling-agnostic.** References to specific vendors are replaced with functional descriptions (e.g., "centralized log management platform" instead of "OpenObserve").

### Guides/

Vendor-neutral tool evaluation guides and implementation roadmaps.

- [Getting Started](Guides/Getting-Started.md) - Three-tier implementation roadmap (Startup, Growth, Enterprise) with policies, tools, costs, and timelines
- [Framework Mapping](Guides/Framework-Mapping.md) - Every policy mapped to NIST 800-53, SOC 2 CC criteria, and ISO 27001 Annex A
- [Log Management](Guides/Log-Management.md) - OpenObserve, Loki, Elastic, Splunk, Datadog
- [SIEM and Security Monitoring](Guides/SIEM-Security-Monitoring.md) - Wazuh, Elastic Security, cloud-native, managed SOC
- [Endpoint Protection](Guides/Endpoint-Protection.md) - OS-native, EDR tiers, MDM integration
- [Password Managers](Guides/Password-Managers.md) - Bitwarden, 1Password, Keeper, browser-native
- [GRC Automation](Guides/GRC-Automation.md) - Drata, Vanta, Secureframe, manual approach
- [Vulnerability Scanning](Guides/Vulnerability-Scanning.md) - OpenVAS, Nessus, cloud-native, SAST/DAST

### Resources/

Operational templates and checklists for running a security program day-to-day.

- [Vendor Security Questionnaire](Resources/Vendor-Questionnaire.md) - Standardized assessment for evaluating third-party vendors
- [Risk Register Template](Resources/Risk-Register-Template.md) - Likelihood × impact scoring with example risks
- [Annual Review Checklist](Resources/Annual-Review-Checklist.md) - Step-by-step checklist for annual policy review
- [Evidence Collection Guide](Resources/Evidence-Collection-Guide.md) - What auditors want per control area, and how to collect it

## Policies Included

| # | Policy | Folder |
|---|--------|--------|
| ISP-001 | Information Security Policy | `Information-Security-Policy/` |
| ISP-002 | Acceptable Use Policy | `Acceptable-Use-Policy/` |
| ISP-003 | AI Usage Policy | `AI-Policy/` |
| ISP-004 | Asset Management Policy | `Asset-Management-Policy/` |
| - | Backup Policy | `Backup-Policy/` |
| - | Business Continuity Plan | `Business-Continuity-Plan/` |
| - | Change Management Policy | `Change-Management-Policy/` |
| - | Code of Conduct | `Code-of-Conduct/` |
| - | Configuration Management Plan | `Configuration-Management-Plan/` |
| - | Data Classification Policy | `Data-Classification-Policy/` |
| - | Data Protection Policy | `Data-Protection-Policy/` |
| - | Data Retention Policy | `Data-Retention-Policy/` |
| - | Disaster Recovery Plan | `Disaster-Recovery-Plan/` |
| - | Encryption Policy | `Encryption-Policy/` |
| - | Network Firewall Policy | `Network-Firewall-Policy/` |
| - | Incident Response Policy | `Incident-Response-Policy/` |
| - | Logging and Monitoring Policy | `Logging-Monitoring-Policy/` |
| - | Password Policy | `Password-Policy/` |
| - | Physical Security Policy | `Physical-Security-Policy/` |
| - | Removable Media Policy | `Removable-Media-Policy/` |
| - | Responsible Disclosure Policy | `Responsible-Disclosure-Policy/` |
| - | Risk Assessment Policy | `Risk-Assessment-Policy/` |
| - | SDLC Policy | `SDLC-Policy/` |
| - | System Access Control Policy | `System-Access-Control-Policy/` |
| - | Vendor Management Policy | `Vendor-Management-Policy/` |
| - | Vulnerability Management Policy | `Vulnerability-Management-Policy/` |

Plus companion processes: Backup Integrity Testing, Disaster Recovery Process, Incident Response Process, Logging and Monitoring Process, Vulnerability Management Process, and more.

## Frameworks Mapped

- **NIST SP 800-53 Rev 5** - Security and Privacy Controls
- **SOC 2** - Trust Services Criteria (CC series)
- **ISO 27001:2022** - Annex A controls
- **NIST CSF 2.0** - Govern, Identify, Protect, Detect, Respond, Recover
- **NIST SP 800-63B** - Digital Identity Guidelines (password policy)

## Usage

1. Browse the `Policy-Templates/` directory and find the policy you need.
2. Read the README first - it explains the purpose, gotchas, and implementation strategy.
3. Copy the template and replace all `____` placeholders with your organization's specifics.
4. Review with your legal team before publishing.

## License

MIT - use freely, attribution appreciated.

## Contributing

Found a gotcha that should be in a README? Have a policy template to add? PRs welcome. Open an issue first to discuss scope.
