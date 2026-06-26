# Software Development Life Cycle Policy

Policy Title: Software Development Life Cycle (SDLC) Policy
Policy Number: ISP-016
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy defines the requirements for a consistent, repeatable software development lifecycle that integrates information security at every phase — from initial requirements through deployment and maintenance.

## Scope

This policy applies to all software development at ____, whether developed internally, outsourced, or acquired from third parties. It applies to all Personnel involved in software development, testing, and deployment.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Annual review; security requirements validation |
| ____ (Engineering Lead) | SDLC process adherence; secure coding standards |
| Developers | Implementation of security controls; secure coding practices |
| ____ (QA/Testing) | Security testing; vulnerability validation |

## Policy

### SDLC Phases

All software development must follow these phases:

#### 1. Requirements Phase

- Security requirements must be identified at the start of each project.
- Threat modeling must be conducted for projects handling sensitive data or exposed to untrusted networks.
- Regulatory, compliance, and contractual security requirements must be documented.
- A data classification analysis must determine the sensitivity of data the system will process.

#### 2. Design Phase

- Security architecture review must be conducted before development begins.
- The principle of least privilege must be applied to component design.
- Data flows must be documented and reviewed for security implications.
- Third-party components and libraries must be evaluated for security posture.

#### 3. Development Phase

- Developers must follow secure coding standards (OWASP Secure Coding Practices or equivalent).
- Code must be managed in a version control system with branch protection.
- All code changes must undergo peer review before merging.
- Peer review must include security considerations, not just functionality.
- Secrets, credentials, and sensitive configuration must never be stored in source code.
- Static Application Security Testing (SAST) must be integrated into the development workflow.
- Dependency scanning must identify known vulnerabilities in third-party libraries.

#### 4. Testing Phase

- Security testing must be performed before production deployment:
  - SAST (static analysis) — automated in CI/CD pipeline.
  - DAST (dynamic analysis) — for web applications and APIs.
  - Dependency vulnerability scanning — continuous.
  - Penetration testing — for significant releases or at least annually.
- Test environments must mirror production as closely as possible.
- Production data must not be used in test environments. Where necessary, data must be anonymized, tokenized, or synthetically generated.
- Test data and accounts must be removed before production deployment.
- QA testing must include edge cases, boundary conditions, and negative testing (invalid/unexpected input).

#### 5. Deployment Phase

- Deployment to production must follow the Change Management Policy.
- Separation of duties must be enforced: developers must not have direct production deployment access without approval.
- Code must be deployed through automated CI/CD pipelines where possible.
- Security controls (WAF, authentication, encryption) must be verified post-deployment.
- A rollback plan must exist before every deployment.

#### 6. Maintenance Phase

- Vulnerabilities discovered post-deployment must be managed per the Vulnerability Management Policy.
- Security patches must be applied within the timeframes defined by the Vulnerability Management Policy.
- End-of-life components must be identified and replaced before vendor support ends.

### Secure Coding Standards

All developers must:

- Complete annual secure coding training, including OWASP Top 10 awareness.
- Validate and sanitize all user input. Never trust client-side validation alone.
- Use parameterized queries or ORM frameworks to prevent SQL injection.
- Encode output to prevent cross-site scripting (XSS).
- Implement proper authentication and authorization checks on every protected endpoint.
- Use approved cryptographic libraries — never implement custom cryptography.
- Handle errors securely: error messages must not leak sensitive information.
- Log security-relevant events (authentication attempts, access control decisions, input validation failures).

### Separation of Environments

- Development, staging/testing, and production environments must be logically or physically separated.
- The level of separation must be appropriate to the sensitivity of the data and application.
- Developer access to production must be restricted per the System Access Control Policy.
- Automated deployment pipelines must enforce separation (no direct developer-to-production pushes).

### Outsourced Development

When software development is outsourced:

- The Vendor Management Policy applies to the development vendor.
- Contractual requirements for secure design, coding, and testing must be included.
- The organization must provide threat models and security requirements to the vendor.
- The organization must verify security testing results, not just accept the vendor's attestation.
- Source code escrow arrangements must be considered for business continuity.
- The organization retains responsibility for compliance and security of the delivered software.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy. Software deployed without following SDLC security requirements may be rolled back or taken offline.

## Related Documents

- Information Security Policy (ISP-001)
- Change Management Policy
- Vulnerability Management Policy
- Vendor Management Policy
- System Access Control Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
