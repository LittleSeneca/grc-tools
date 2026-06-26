# Policy Templates

Production-ready information security policy templates aligned with NIST 800-53, SOC 2, and ISO 27001. Every template follows the ISP (Information Security Program) house format.

## Document Structure

Each policy folder contains:

- **Policy template (`Template.md`):** The policy document — defines WHAT is required. Written for a broad audience (employees, auditors, executives). Uses ISP house format with `____` placeholders.
- **Companion procedures (`*-Procedures.md`):** Implementation guidance — describes HOW to operationalize the policy. Written for implementers (IT, security, HR teams). Each procedure includes standard approaches, alternative methods (with rationale), and common pitfalls.
- **README (`README.md`):** Plain-English overview, gotchas, implementation advice, and an explanation of the policy/procedure split.

Policies and procedures are deliberately separated: policies change infrequently and are approved by leadership; procedures evolve with tools and operational experience. Keeping them separate prevents version-control chaos and makes both documents more usable for their intended audiences.

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
