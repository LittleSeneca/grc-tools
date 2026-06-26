# Software Development Life Cycle (SDLC) Policy

Policy Title: Software Development Life Cycle Policy
Policy Number: ISP-____
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy defines the high-level requirements for providing business program managers, project managers, technical leads, and other stakeholders with guidance to support the approval, planning, and lifecycle development of ____ software systems, aligned with the Information Security Program. It ensures that information security is integrated into every phase of the software development lifecycle.

## Scope

This policy applies to:

- All software developed by or for ____, including internal applications, customer-facing applications, APIs, mobile applications, and system utilities.
- All software development methodologies employed (Waterfall, Agile, Iterative, DevOps, etc.).
- All personnel involved in the software development lifecycle, including developers, architects, quality assurance testers, project managers, and system owners.
- Software developed by third parties or outsourced development teams on behalf of ____.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (e.g., Security Officer) | Annual policy review and updates; security requirements oversight |
| ____ (e.g., Privacy Officer / Data Protection Officer) | Approval of systems handling personal or sensitive data |
| Development Managers / Tech Leads | Ensuring SDLC security requirements are implemented within their teams |
| Developers | Secure coding practices; code review participation; remediation of security findings |
| QA / Testers | Security testing; verification of security requirements |
| Product Managers / System Owners | Defining security requirements during planning; accepting residual risk |

## Policy

____ must establish and maintain processes for ensuring that all computer applications and systems follow an SDLC process that is consistent, repeatable, and maintains information security at every stage. Security must be integrated from the earliest phases ("shift left") rather than being added as an afterthought.

## Software Development Phases and Security Integration

A Software Development Project consists of a defined set of phases. The sequence and iteration of these phases depends on the development methodology, but security activities are required at each phase regardless of methodology.

### Phase 1: Determine System Need

**Activities:** An information system need is identified, and a decision is made whether to commit resources.

**Security Activities:**

- Conduct an initial security impact assessment: will this system process, store, or transmit sensitive or regulated data?
- Identify applicable regulatory and compliance requirements (GDPR, HIPAA, PCI DSS, SOC 2, etc.).
- Determine the system's information classification level.
- Identify external dependencies and third-party components that may introduce supply chain risk.
- Document security and privacy requirements at a high level for inclusion in the business case.

### Phase 2: Define System Requirements

**Activities:** User requirements are decomposed into detailed functional and non-functional requirements.

**Security Activities:**

- Conduct a formal information security risk assessment for the proposed system.
- Define detailed security requirements including:
  - Authentication and authorization requirements.
  - Data protection requirements (encryption at rest and in transit).
  - Audit logging and monitoring requirements.
  - Input validation and output encoding requirements.
  - Session management requirements.
  - Error handling and exception management requirements.
  - API security requirements.
- Identify security controls that must be inherited from the organizational security framework.
- Document security requirements in the system requirements specification.
- Privacy review and approval (if handling personal data).

### Phase 3: Design System Components

**Activities:** Requirements are transformed into architectural and detailed design specifications.

**Security Activities:**

- Conduct threat modeling to identify potential threats and attack vectors against the system design.
  - Use structured methodologies such as STRIDE, PASTA, or attack trees.
  - Document identified threats, their likelihood and impact, and planned mitigations.
- Perform architecture risk analysis.
- Design security controls into the system architecture:
  - Authentication and authorization architecture.
  - Encryption architecture (key management, certificate management).
  - Network segmentation and API gateway design.
  - Logging and monitoring architecture.
- Apply secure design principles:
  - Defense in depth (multiple layers of security controls).
  - Least privilege (minimal access required for functionality).
  - Secure by default (secure out-of-the-box configurations).
  - Fail secure (failures default to secure state).
  - Economy of mechanism (keep design simple and verifiable).
  - Complete mediation (validate all access requests).
  - Open design (security should not depend on secrecy of design).
  - Separation of duties.
  - Psychological acceptability (security should not make the system unusable).
- Review data flow diagrams to identify trust boundaries and data sensitivity at each boundary.

### Phase 4: Build System Components

**Activities:** Detailed design is transformed into coded software units, integrated into a complete product.

**Security Activities:**

- **Secure Coding Standards:** All developers must follow secure coding practices aligned with the **OWASP Top 10** (current version) and relevant language-specific secure coding guidelines (e.g., OWASP Secure Coding Practices, CERT secure coding standards).
- **Developer Security Training:** All developers must complete annual secure coding training, including OWASP Top 10 awareness. Developers working on internet-facing applications must receive additional training on internet-specific threats. Training must be completed before writing or supporting code for production systems.
- **Automated Security Testing (CI/CD Pipeline):**
  - **Static Application Security Testing (SAST):** Automated code analysis integrated into the build pipeline to identify security vulnerabilities before code is merged.
  - **Software Composition Analysis (SCA):** Automated scanning of open-source dependencies and third-party libraries for known vulnerabilities (CVEs).
  - **Container Image Scanning:** Automated scanning of container images for vulnerabilities before deployment.
  - **Infrastructure-as-Code (IaC) Scanning:** Automated scanning of infrastructure configuration templates for misconfigurations.
  - Pipeline must be configured to fail builds for Critical and High severity findings, with appropriate exception processes for documented and approved waivers.
- **Code Review:**
  - All code changes must be reviewed by at least one individual other than the originating author.
  - Reviewers must be knowledgeable in code review techniques and secure coding practices.
  - Code reviews must include review for security vulnerabilities, not just functional correctness.
  - Overrides of edit checks, approvals, and changes to confirmed transactions must be appropriately authorized, documented, and reviewed.
- **Secrets Management:**
  - Secrets (API keys, database credentials, encryption keys, tokens) must never be hardcoded in source code, configuration files, or committed to version control.
  - Secrets must be managed through a dedicated secrets management system (e.g., HashiCorp Vault, AWS Secrets Manager, Azure Key Vault).
  - Credential scanning tools must be used to detect and prevent accidental secret exposure in code repositories.
- **Environment Separation:**
  - Development, testing, and production environments must be separated (logically or physically, based on risk assessment).
  - Access to production environments must be restricted and controlled through change management.
  - Development and testing activities must not occur in production environments.

### Phase 5: Evaluate System Readiness

**Activities:** The system is tested to verify it satisfies functional, performance, and security requirements.

**Security Activities:**

- **Security Testing:**
  - **Dynamic Application Security Testing (DAST):** Automated scanning of running applications for vulnerabilities.
  - **Penetration Testing:** For high-risk or internet-facing applications, engage qualified security testers for manual penetration testing.
  - **API Security Testing:** Validate API authentication, authorization, input validation, rate limiting, and data exposure.
  - **Authentication and Authorization Testing:** Verify that access controls function correctly for all user roles.
  - **Fuzz Testing:** Where appropriate, test application resilience against malformed or unexpected inputs.
- **Quality Assurance Testing:** Feature acceptance tests must include testing of edge cases, boundary conditions, and negative test cases in addition to normal-path testing.
- **Security Acceptance Criteria:** The system must meet all defined security requirements before being approved for deployment. Any exceptions must be formally documented with risk acceptance.
- **Independent Testing:** Where possible, security testing should be performed by testers independent of the development team.

### Phase 6: System Deployment

**Activities:** The system is released initially to a pilot or staging environment and then into production.

**Security Activities:**

- **Pre-Deployment Checklist:**
  - All Critical and High security findings resolved or formally accepted.
  - Security configuration hardened per organizational baselines.
  - Default accounts and passwords removed or changed.
  - Test data, test accounts, and debug functionality removed from production deployment.
  - Production data NOT present in the deployment artifacts (code, configuration).
  - Encryption configured and verified (TLS certificates, database encryption, secrets).
  - Logging configured and verified to forward to centralized logging platform.
  - Monitoring and alerting configured for the new system.
- **Deployment Automation:** Deployments must follow a defined, repeatable process (CI/CD pipeline) with human approval gates for production deployments.
- **Change Management:** All production deployments must follow the change management process, with appropriate approval based on risk.
- **Post-Deployment Monitoring:** Monitor the system closely for security events, errors, and performance issues for at least ____ days after deployment.
- **User Training:** Provide security training to users of the new system covering appropriate use, data handling, and incident reporting.

### Project Management

The sequence and iteration of development phases depends on the project management approach:

- **Waterfall:** Sequential phases with formal gate reviews between each phase.
- **Agile / Scrum:** Iterative cycles (sprints) with security activities integrated into each sprint's definition of done.
- **DevOps / Continuous Delivery:** Security activities integrated into the CI/CD pipeline (DevSecOps), with automated security gates.
- **Staged Delivery:** Phases may overlap across multiple releases; security requirements apply to each release increment.

Regardless of methodology, security activities must be traceable from requirements through design, implementation, testing, and deployment.

## SDLC Security Controls — Detailed Requirements

### Production Data in Non-Production Environments

Production data must NOT be used in development or testing environments. If a specific situation requires the use of production data for testing:

- The Information Resource Owner must provide written approval before production data can be used for testing.
- Production data must be tokenized, anonymized, pseudonymized, or de-identified wherever possible.
- A separate copy of production data must be used; the test location must be acceptable (e.g., production data on a developer laptop is NOT acceptable).
- Data must not be extracted, handled, or used in a manner that subjects it to unauthorized disclosure.
- Access to the test data must follow the same access control procedures as the production environment (need-to-know basis).
- Each copy of production data to a test environment requires separate authorization.
- Copying production data to test environments must be logged for an audit trail.
- Access to production data in test environments must be restricted to individuals with a documented business need.
- Production data used for testing must be securely erased upon completion of testing.
- Test data, test accounts, and test credentials must be removed before systems are placed into production.

### Third-Party and Outsourced Development

If software development is outsourced to third parties, the following must be addressed across the entire external supply chain, in conjunction with the Vendor Management Policy:

- Licensing arrangements, code ownership, and intellectual property rights related to outsourced content.
- Contractual requirements for secure design, coding, and testing practices.
- Provision of the approved threat model to the external developer.
- Acceptance testing for quality, accuracy, and security of deliverables.
- Evidence that security thresholds were used to establish minimum acceptable levels of security and privacy quality.
- Evidence that sufficient testing has been applied to guard against intentional and unintentional malicious content (backdoors, logic bombs).
- Evidence that sufficient testing has been applied to guard against known vulnerabilities.
- Source code escrow arrangements (if source code becomes unavailable).
- Contractual right to audit development processes and controls.
- Effective documentation of the build environment used to create deliverables.
- ____ remains responsible for compliance with applicable laws and control effectiveness verification, even when development is outsourced.

### Error Handling and Logging

- Error messages must not leak sensitive information (stack traces, internal IP addresses, database schema, API keys).
- User-facing error messages must be generic; detailed error information must be logged server-side for debugging.
- All security-relevant events must be logged in accordance with the Logging and Monitoring Policy.

### Encryption

- Restricted, Confidential, and Protected information must be encrypted at rest and in transit per the Encryption Policy.
- Encryption algorithms and key lengths must meet current industry standards (avoid deprecated algorithms like MD5, SHA-1, DES, 3DES, RC4).
- TLS must be configured to current best practices (disable older protocols and weak cipher suites).

### Change Control

- All changes to production environments must strictly follow change control procedures.
- All changes must have human approval granted by an authorized owner of that environment.
- Automated updates must NOT be allowed without such approval (except for pre-approved automated security patching within defined parameters).
- Emergency changes must follow an expedited process with post-change review.

## Compliance and Enforcement

Violation of this policy, including failure to follow secure coding practices, failure to complete required security testing, or deployment of code containing Critical unaddressed security findings, may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- Change Management Policy
- Vulnerability Management Policy
- Encryption Policy
- Logging and Monitoring Policy
- Data Classification Policy
- Vendor Management Policy
- Risk Assessment Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
