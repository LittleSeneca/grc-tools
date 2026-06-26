# Policy Templates

Production-ready information security policy templates aligned with NIST 800-53, SOC 2, and ISO 27001. Every template follows the ISP (Information Security Program) house format.

## Document Structure

Each policy folder contains:

- **Policy template (`Template.md` or `*-Template.md`):** The policy document — defines WHAT is required. Written for a broad audience (employees, auditors, executives). Uses ISP house format with `____` placeholders.
- **Companion procedures (`*-Procedures.md`):** Implementation guidance — describes HOW to operationalize the policy. Written for implementers (IT, security, HR teams). Each procedure includes standard approaches, alternative methods (with rationale), and common pitfalls.
- **README (`README.md`):** Plain-English overview, gotchas, implementation advice, and an explanation of the policy/procedure split.

Policies and procedures are deliberately separated: policies change infrequently and are approved by leadership; procedures evolve with tools and operational experience. Keeping them separate prevents version-control chaos and makes both documents more usable for their intended audiences.

### Policies with Companion Procedures

| Policy | Template | Procedures | Procedures Extracted |
|--------|----------|------------|---------------------|
| Acceptable Use Policy | AUP-Template.md | AUP-Procedures.md | Monitoring, enforcement, BYOD, data handling |
| AI Usage Policy | AI-Template.md | AI-Procedures.md | Platform registration, data handling, model governance |
| Asset Management Policy | AMP-Template.md | Asset-Management-Procedures.md | System hardening, patch management, media transfer, media disposal, sanitization validation |
| Change Management Policy | Template.md | Change-Management-Procedures.md | Software dev change process, infrastructure/config change management, CAB operations |
| Data Protection Policy | Template.md | Data-Protection-Procedures.md | Customer data protection, production access controls, monitoring setup, secure data deletion, data transfer authorization |
| Information Security Policy | ISP-Template.md | ISP-Procedures.md | Risk assessment methodology, policy exception handling, annual review process |
| SDLC Policy | SDLC-Template.md | SDLC-Procedures.md | Requirements & risk assessment, threat modeling, secure coding & build, security testing, deployment & production readiness |
| System Access Control Policy | Template.md | System-Access-Control-Procedures.md | Access provisioning, access reviews, offboarding & revocation, privileged access management |

## How to Use

1. Read the README in the policy folder first — it explains the policy's purpose, common mistakes, and implementation strategy.
2. Copy the policy template and replace all `____` placeholders with your organization's specifics.
3. Copy the companion procedures and adapt the approaches to your environment — the alternatives section helps you choose what fits.
4. Have legal review the final policy document.
5. Publish the policy to your policy management platform. Keep the procedures accessible to the teams that need them.

## Templates

### Core Governance

| Policy | Folder | Description |
|--------|--------|-------------|
| Information Security Policy | [`Information-Security-Policy/`](./Information-Security-Policy/) | Overarching ISP framework — the constitution all other policies derive from |
| Code of Conduct | [`Code-of-Conduct/`](./Code-of-Conduct/) | Ethical standards, conflicts of interest, confidentiality expectations |
| Responsible Disclosure Policy | [`Responsible-Disclosure-Policy/`](./Responsible-Disclosure-Policy/) | Vulnerability reporting and whistleblower protection |

### Data Protection

| Policy | Folder | Description |
|--------|--------|-------------|
| Data Classification Policy | [`Data-Classification-Policy/`](./Data-Classification-Policy/) | Data sensitivity levels and handling controls per classification |
| Data Protection Policy | [`Data-Protection-Policy/`](./Data-Protection-Policy/) | Technical controls for protecting data at rest, in transit, and in use |
| Data Retention Policy | [`Data-Retention-Policy/`](./Data-Retention-Policy/) | Retention periods and deletion requirements |

### Access and Identity

| Policy | Folder | Description |
|--------|--------|-------------|
| System Access Control Policy | [`System-Access-Control-Policy/`](./System-Access-Control-Policy/) | Role-based access, provisioning, reviews, termination |
| Password Policy | [`Password-Policy/`](./Password-Policy/) | Password complexity, MFA, and credential management |
| Acceptable Use Policy | [`Acceptable-Use-Policy/`](./Acceptable-Use-Policy/) | Rules for using company devices, networks, and data |

### Asset and Endpoint Security

| Policy | Folder | Description |
|--------|--------|-------------|
| Asset Management Policy | [`Asset-Management-Policy/`](./Asset-Management-Policy/) | Asset lifecycle tracking, hardening, disposal |
| Encryption Policy | [`Encryption-Policy/`](./Encryption-Policy/) | Cryptographic controls, key management, full-disk encryption |
| Physical Security Policy | [`Physical-Security-Policy/`](./Physical-Security-Policy/) | Facility access, workstation security, off-site equipment |
| Removable Media Policy | [`Removable-Media-Policy/`](./Removable-Media-Policy/) | Restrictions on USB drives and portable storage |

### Risk and Vulnerability Management

| Policy | Folder | Description |
|--------|--------|-------------|
| Risk Assessment Policy | [`Risk-Assessment-Policy/`](./Risk-Assessment-Policy/) | Risk identification, impact/likelihood scoring, treatment |
| Vulnerability Management Policy | [`Vulnerability-Management-Policy/`](./Vulnerability-Management-Policy/) | Scanning, prioritization, remediation SLAs |
| Vulnerability Management Process | [`Vulnerability-Management-Process/`](./Vulnerability-Management-Process/) | Operational process for managing findings |
| Remediation Plan | [`Remediation-Plan/`](./Remediation-Plan/) | Vulnerability remediation tracking and verification |

### Incident Response and Monitoring

| Policy | Folder | Description |
|--------|--------|-------------|
| Incident Response Policy | [`Incident-Response-Policy/`](./Incident-Response-Policy/) | IR team, detection mechanisms, response phases |
| Incident Response Process | [`Incident-Response-Process/`](./Incident-Response-Process/) | Step-by-step IR procedures |
| Logging and Monitoring Policy | [`Logging-Monitoring-Policy/`](./Logging-Monitoring-Policy/) | Audit log requirements, monitoring, log protection |
| Logging and Monitoring Process | [`Logging-Monitoring-Process/`](./Logging-Monitoring-Process/) | Log collection, retention, alerting configuration |

### Business Continuity

| Policy | Folder | Description |
|--------|--------|-------------|
| Backup Policy | [`Backup-Policy/`](./Backup-Policy/) | Backup requirements, frequency, encryption |
| Backup Integrity Testing Process | [`Backup-Integrity-Testing-Process/`](./Backup-Integrity-Testing-Process/) | Testing that backups can actually be restored |
| Business Continuity Plan | [`Business-Continuity-Plan/`](./Business-Continuity-Plan/) | Continuity procedures and response teams |
| Disaster Recovery Plan | [`Disaster-Recovery-Plan/`](./Disaster-Recovery-Plan/) | System recovery procedures, RTO/RPO targets |
| Disaster Recovery Process | [`Disaster-Recovery-Process/`](./Disaster-Recovery-Process/) | Operational DR procedures |

### Development and Change

| Policy | Folder | Description |
|--------|--------|-------------|
| SDLC Policy | [`SDLC-Policy/`](./SDLC-Policy/) | Secure development lifecycle, OWASP, code review |
| Change Management Policy | [`Change-Management-Policy/`](./Change-Management-Policy/) | Change approval, CAB, deployment controls |
| Configuration Management Plan | [`Configuration-Management-Plan/`](./Configuration-Management-Plan/) | CM baselines, libraries, release management |

### Third-Party and Emerging Tech

| Policy | Folder | Description |
|--------|--------|-------------|
| Vendor Management Policy | [`Vendor-Management-Policy/`](./Vendor-Management-Policy/) | Vendor risk assessment, contract security requirements |
| AI Usage Policy | [`AI-Policy/`](./AI-Policy/) | Approved AI platforms, data handling, prohibited use |
| Network Firewall Policy | [`Network-Firewall-Policy/`](./Network-Firewall-Policy/) | Firewall deployment, ruleset management, review |

### Personnel

| Policy | Folder | Description |
|--------|--------|-------------|
| Employee Access Agreement | [`Employee-Access-Agreement/`](./Employee-Access-Agreement/) | Confidentiality obligations for employees |
| Contractor Access Agreement | [`Contractor-Access-Agreement/`](./Contractor-Access-Agreement/) | Confidentiality obligations for contractors |

### Operational Processes

| Process | Folder | Description |
|---------|--------|-------------|
| Database Password Management | [`Database-Password-Management/`](./Database-Password-Management/) | Database credential rotation procedures |
| Control Self Assessment | [`Control-Self-Assessment/`](./Control-Self-Assessment/) | Control effectiveness self-evaluation process |

### Implementation Procedures

> Companion procedure documents that provide step-by-step implementation guidance, alternatives, and common pitfalls for the corresponding policies above. These are separated from the policy templates to keep policies concise and focused on requirements.

| Procedures | Policy | Folder |
|------------|--------|--------|
| ISP Implementation Procedures | Information Security Policy | [`Information-Security-Policy/ISP-Procedures.md`](./Information-Security-Policy/ISP-Procedures.md) |
| AUP Implementation Procedures | Acceptable Use Policy | [`Acceptable-Use-Policy/AUP-Procedures.md`](./Acceptable-Use-Policy/AUP-Procedures.md) |
| Asset Management Implementation Procedures | Asset Management Policy | [`Asset-Management-Policy/Asset-Management-Procedures.md`](./Asset-Management-Policy/Asset-Management-Procedures.md) |
| Data Protection Implementation Procedures | Data Protection Policy | [`Data-Protection-Policy/Data-Protection-Procedures.md`](./Data-Protection-Policy/Data-Protection-Procedures.md) |
| System Access Control Implementation Procedures | System Access Control Policy | [`System-Access-Control-Policy/System-Access-Control-Procedures.md`](./System-Access-Control-Policy/System-Access-Control-Procedures.md) |
| Change Management Implementation Procedures | Change Management Policy | [`Change-Management-Policy/Change-Management-Procedures.md`](./Change-Management-Policy/Change-Management-Procedures.md) |
| SDLC Implementation Procedures | SDLC Policy | [`SDLC-Policy/SDLC-Procedures.md`](./SDLC-Policy/SDLC-Procedures.md) |
| Backup Implementation Procedures | Backup Policy | [`Backup-Policy/Backup-Procedures.md`](./Backup-Policy/Backup-Procedures.md) |
| BCP Implementation Procedures | Business Continuity Plan | [`Business-Continuity-Plan/BCP-Procedures.md`](./Business-Continuity-Plan/BCP-Procedures.md) |
| DR Implementation Procedures | Disaster Recovery Plan | [`Disaster-Recovery-Plan/DR-Procedures.md`](./Disaster-Recovery-Plan/DR-Procedures.md) |
| VM Implementation Procedures | Vulnerability Management Policy | [`Vulnerability-Management-Policy/Vulnerability-Management-Procedures.md`](./Vulnerability-Management-Policy/Vulnerability-Management-Procedures.md) |
| IR Implementation Procedures | Incident Response Policy | [`Incident-Response-Policy/IR-Procedures.md`](./Incident-Response-Policy/IR-Procedures.md) |
| AI Implementation Procedures | AI Usage Policy | [`AI-Policy/AI-Procedures.md`](./AI-Policy/AI-Procedures.md) |
| Encryption Implementation Procedures | Encryption Policy | [`Encryption-Policy/Encryption-Procedures.md`](./Encryption-Policy/Encryption-Procedures.md) |
