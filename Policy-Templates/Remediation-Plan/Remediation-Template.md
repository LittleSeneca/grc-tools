# Remediation Plan

Policy Title: Remediation Plan
Policy Number: ISP-____
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Introduction

This Remediation Plan document is designed to guide the remediation of vulnerabilities and security findings identified within ____'s digital environment. It is structured against the Vulnerability Management Policy and NIST SP 800-53 standards, and defines the end-to-end workflow for tracking and managing remediation efforts from discovery through verification and closure.

## Objective

The objective of this plan is to ensure that all identified vulnerabilities, misconfigurations, and security findings are addressed promptly and efficiently, reducing risk to ____'s operations and maintaining the confidentiality, integrity, and availability of organizational and customer data.

## Remediation Process Overview

The remediation process follows a structured workflow:

1. **Identification:** Vulnerabilities and findings are identified through automated tools, manual investigations, or external assessments and documented in a work tracking system.
2. **Prioritization:** Findings are prioritized based on severity scoring, business impact analysis, and exploitability.
3. **Assignment:** Each finding is assigned to the responsible team or individual based on asset ownership and the nature of the vulnerability.
4. **Remediation Action:** The assigned team applies the appropriate risk treatment option (Resolve, Mitigate, Transfer, or Accept).
5. **Verification and Closure:** Remediation is verified, documented, and the finding is formally closed.
6. **Review and Reporting:** Aggregated remediation outcomes are reviewed by the governance body and used to improve the overall security program.

## Detailed Remediation Workflow

### 1. Identification

**Sources of Findings:**

Vulnerabilities and security findings are identified through multiple channels:

- **Automated Scanning:** Infrastructure vulnerability scanners, web application scanners, container/image scanners, CSPM tools, and dependency scanning tools.
- **Manual Investigation:** Security team reviews, threat hunting, code reviews, and architecture reviews.
- **External Assessment:** Penetration testing reports, third-party security audits, and bug bounty submissions.
- **Incident Response:** Findings discovered during or after security incidents.

**Documentation Requirements:**

Each finding must be documented in the designated work tracking platform with the following minimum information:

- Unique identifier (ticket/task ID).
- Finding description (what was discovered, where, and by what means).
- Severity classification (Critical, High, Medium, Low).
- Affected asset(s) and asset owner.
- Date of discovery.
- Detection source (tool name, manual review, external report).
- Relevant industry identifiers (CVE, CWE, vendor advisory).
- CVSS score (base, and where applicable, temporal and environmental).
- Initial remediation guidance or recommendations.
- Any associated screenshots, logs, or evidence.

**Automation:**

Where possible, automation is used for:

- Auto-creation of tickets from scanning tool findings via API integration.
- Auto-enrichment of tickets with CVE details, CVSS scores, and known remediation guidance.
- Auto-assignment based on asset ownership mappings.

### 2. Prioritization

**Prioritization Factors:**

Findings are prioritized based on a combination of:

1. **CVSS Score:** The base severity score from the Common Vulnerability Scoring System.
2. **Asset Criticality:** The business importance of the affected asset (Critical / High / Medium / Low classification from the asset inventory).
3. **Exploitability:** Whether a known public exploit exists, whether the vulnerability is actively being exploited in the wild (e.g., listed in CISA Known Exploited Vulnerabilities catalog), and the complexity of exploitation.
4. **Exposure:** Whether the vulnerable asset is internet-facing, accessible from less-trusted network zones, or requires authenticated access.
5. **Data Sensitivity:** The classification level of data that could be compromised.

**Severity Classification:**

| Severity | Criteria | Examples |
|----------|----------|----------|
| **Critical** | CVSS 9.0–10.0, OR active exploitation in the wild, OR critical asset with sensitive data exposure | Remote code execution on internet-facing production server; known exploited vulnerability on critical infrastructure |
| **High** | CVSS 7.0–8.9, OR high business impact with moderate exploitability | Authentication bypass on internal application; privilege escalation on production database |
| **Medium** | CVSS 4.0–6.9, OR moderate business impact | Information disclosure of non-sensitive data; missing security headers |
| **Low** | CVSS 0.1–3.9, OR minimal business impact | Low-severity configuration issues; informational findings with no direct exploit path |

### 3. Assignment

**Assignment Criteria:**

Findings are assigned based on:

- **Asset Ownership:** The team or individual responsible for the affected system.
- **Vulnerability Type:**
  - **Code vulnerabilities** (SAST/DAST findings, dependency vulnerabilities) → Development team.
  - **Infrastructure vulnerabilities** (missing patches, misconfigurations) → Infrastructure/DevOps team.
  - **Cloud misconfigurations** → Cloud engineering / platform team.
  - **Network vulnerabilities** → Network engineering team.
  - **Application configuration issues** → Application support / DevOps team.

**Assignment SLAs:**

| Severity | Assignment Target |
|----------|------------------|
| Critical | Within 4 hours of detection |
| High | Within 24 hours of detection |
| Medium | Within 3 business days |
| Low | Within 5 business days |

**Escalation:**

If an assigned ticket is not acknowledged within the assignment SLA:

1. Escalate to the team lead.
2. If not acknowledged within an additional time period, escalate to the Security Officer.
3. Escalation paths are documented in the ticketing system workflow.

### 4. Remediation Action

Each finding is addressed using one of four risk treatment options:

#### Resolve (Preferred)

Full remediation of the vulnerability:

- Apply vendor patches or updates.
- Update configurations to secure baselines.
- Fix vulnerable code.
- Upgrade or replace end-of-life software/components.
- Decommission unused or unsupported assets.

#### Mitigate

Implement compensating controls to reduce risk when full remediation is not immediately feasible:

- Network segmentation to limit access to the vulnerable asset.
- Web application firewall (WAF) rules to block exploitation attempts.
- Disable vulnerable features or modules.
- Restrict access to the affected system (IP allowlisting, reduced permissions).
- Increase monitoring and alerting on the affected system.

**Mitigation Requirements:**

- Mitigations must be documented with a clear description of the control implemented.
- Every mitigation must have a plan and target date for eventual resolution.
- Mitigations must be reviewed at least quarterly to ensure they remain effective and to assess whether resolution has become feasible.

#### Transfer

Shift risk to a third party:

- Rely on cyber insurance coverage for the specific risk.
- Transfer liability through contractual arrangements with vendors or service providers.
- Outsource management of the affected system to a provider with contractual security obligations.

**Transfer Requirements:**

- Transfer decisions must be documented with the rationale and the specific third party involved.
- Transfer does not eliminate internal tracking — the finding remains in the system with the treatment recorded as "Transferred."
- Transferred risks must be reviewed when insurance policies or contracts renew.

#### Accept

Formally accept the risk:

Acceptance is only permitted when:

- The risk level is Low, OR
- The cost of remediation demonstrably exceeds the potential impact of exploitation, OR
- No practical remediation, mitigation, or transfer option exists, AND
- The business justification is documented and approved.

**Acceptance Requirements:**

- Acceptance must be formally documented with:
  - Business justification.
  - Cost-benefit analysis (if acceptance is based on remediation cost).
  - Compensating controls in place (if any).
  - Approval by ____ (role-based approval thresholds):
    - **Medium/Low:** Approval by asset owner and security team.
    - **High:** Approval by ____ (e.g., CISO / Security Officer).
    - **Critical:** Approval by executive leadership.
- All accepted risks must be reviewed at least quarterly to validate the acceptance rationale remains valid.
- Accepted risks must be reported to the Security Oversight Committee.

### 5. Verification and Closure

**Verification:**

After remediation actions are completed, verification must be performed before closure:

- **For Resolved findings:** Re-scan the affected asset to confirm the vulnerability is no longer present. For critical vulnerabilities, a targeted manual verification test is recommended.
- **For Mitigated findings:** Test the compensating control to confirm it effectively blocks exploitation. Document how the control was tested.
- **For Transferred findings:** Confirm the transfer arrangement is in place (e.g., insurance policy active, contract executed).
- **For Accepted findings:** Confirm the acceptance documentation is complete and approved.

**Closure:**

A finding is closed when:

- Verification confirms the remediation is effective, OR the risk treatment option is formally documented and approved.
- The closure is documented in the ticketing system with:
  - Date of closure.
  - Verification method and result.
  - Final risk treatment applied.
  - Any follow-up actions required.

**Closure SLAs:**

| Severity | Closure Target (from Discovery) |
|----------|-------------------------------|
| Critical | Within ____ days (recommended: 7 days) |
| High | Within ____ days (recommended: 30 days) |
| Medium | Within ____ days (recommended: 90 days) |
| Low | Within ____ days (recommended: 365 days) |

### 6. Review and Reporting

**Aggregation and Review:**

- All remediation efforts and outcomes are aggregated and reviewed during the ____ (monthly/quarterly) Security Oversight Committee meetings.
- The review covers:
  - Open findings by severity and age.
  - SLA adherence metrics (percentage of findings remediated within target).
  - Aging findings approaching or exceeding SLA.
  - Risk acceptance inventory and aging.
  - Recurring vulnerability types (indicating systemic issues requiring process or architectural changes).
  - Lessons learned from recent remediation efforts.

**Reporting:**

Regular reports on the status of remediation efforts include:

- **Operational Dashboard:** Real-time view in the ticketing system showing open findings, SLA status, and assignment.
- **Monthly Report:** Summary for the Security Oversight Committee.
- **Quarterly Report:** Comprehensive report for senior management, including trends, SLA compliance percentage, and top risks.

Critical vulnerabilities must be communicated immediately to relevant stakeholders upon discovery, not waiting for the next review cycle.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| **Security Analysts / Vulnerability Management Team** | Initial identification, documentation, and prioritization of findings; verification of remediation |
| **IT, Infrastructure, and Development Teams** | Execution of remediation actions as assigned in tickets |
| **Asset Owners** | Approval of remediation plans for their assets; risk acceptance decisions within their authority |
| **Security Officer (or equivalent)** | Oversight of remediation SLAs; approval of High risk acceptances; escalation point for overdue findings |
| **Security Oversight Committee** | Review of aggregated findings; approval of Critical risk acceptances; strategic decisions on systemic issues |
| **Executive Leadership** | Approval of Critical risk acceptances; resource allocation for remediation programs |

## Tools and Resources

The following tool categories support the remediation program. Specific tool selections should be documented here:

- **Work Tracking / Ticketing Platform:** Primary system for tracking, managing, assigning, and reporting on findings and their remediation status (e.g., Jira, ServiceNow, Linear).
- **Centralized Logging and Observability Platform:** Provides visibility into system state and security events to support finding identification and verification (recommended: OpenObserve).
- **Cloud Security Posture Management (CSPM):** Generates and consolidates infrastructure-related security findings.
- **Vulnerability Scanning Tools:** Generate findings related to vulnerabilities in applications, infrastructure, and dependencies.
- **Configuration Management and IaC Tools:** Automate remediation of configuration-based findings (e.g., Ansible, Terraform, Puppet, Chef).
- **Patch Management System:** Automates operating system and application patching.

## Remediation Timeline Summary

| Severity | Assignment | Remediation Target |
|----------|-----------|-------------------|
| **Critical** | Within 4 hours | Within one hot-fix cycle (recommended: ≤ 7 days) |
| **High** | Within 24 hours | Within 30 days |
| **Medium** | Within 3 business days | Within 90 days (one quarter) |
| **Low** | Within 5 business days | Within 365 days (one year) |

## Exception Management

Requests for exceptions to remediation timelines must:

- Be submitted with a written business justification.
- Include compensating controls that will be in place during the exception period.
- Be approved by ____ (Security Officer or equivalent).
- Include a revised target remediation date.
- Be reviewed at each Security Oversight Committee meeting.

## Related Documents

- Vulnerability Management Policy
- Vulnerability Management Process
- Information Security Policy (ISP-001)
- Risk Assessment Policy
- Change Management Policy
- Configuration Management Plan
- Incident Response Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
