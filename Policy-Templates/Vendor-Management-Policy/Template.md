# Vendor Management Policy

Policy Title: Vendor Management Policy
Policy Number: ISP-014
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes requirements for evaluating, onboarding, and managing third-party service providers and vendors to ensure they meet ____ security and privacy standards.

## Scope

This policy applies to all IT vendors, service providers, contractors, and partners who have the ability to impact the confidentiality, integrity, or availability of ____ technology, systems, or sensitive information. It applies to all Personnel responsible for vendor management and oversight.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Annual policy review; vendor security assessments |
| ____ (Procurement / Legal) | Contract negotiation; vendor onboarding |
| Vendor Owners (relationship managers) | Ongoing vendor oversight; risk reassessment |
| Vendors | Compliance with contractual security requirements |

## Policy

### Vendor Risk Assessment

Before engaging a vendor, a risk assessment must be conducted based on:

| Risk Level | Criteria |
|-----------|----------|
| **High** | Vendor stores, processes, or has access to sensitive data (PII, financial data, credentials); a failure would have critical business impact |
| **Medium** | Vendor does not access sensitive data but provides a service that impacts business operations; a failure would have moderate impact |
| **Low** | Vendor has no access to sensitive data and a failure would have minimal business impact |

### Security Requirements by Risk Level

**High-Risk Vendors:**

- Must provide a current third-party audit report (SOC 2 Type 2, ISO 27001, or equivalent). In the absence of a third-party audit, the organization may conduct its own security assessment.
- Must sign a contract that includes security and data protection clauses.
- Must comply with all applicable ____ security policies.
- Must be reviewed annually.

**Medium-Risk Vendors:**

- Must complete a security questionnaire covering key control areas.
- Must sign a contract with appropriate security clauses.
- Must be reviewed biennially or when the service scope changes.

**Low-Risk Vendors:**

- Standard terms and conditions may suffice. No formal security review required.
- Must not be granted access to sensitive systems or data.

### Vendor Inventory

A vendor inventory must be maintained and include:

- Vendor name and primary contact.
- Risk level and date of last assessment.
- Types of data shared with the vendor.
- Brief description of services provided.
- How the vendor accesses organizational systems or data (API, VPN, SSO).
- Key security controls in place.
- Security assessment report or questionnaire on file.
- Contract expiration date.

### Contract Requirements

Contracts with High and Medium-risk vendors must include:

- Acknowledgment that the vendor is responsible for the security of organizational data it possesses, stores, processes, or transmits.
- Requirement for regular security control review and validation.
- Background verification requirements for vendor personnel.
- Incident notification requirements, including timing as defined by SLAs (recommended: within 24 hours for security incidents affecting customer data).
- Data breach responsibilities and liability.
- Data return or secure destruction requirements upon contract termination.
- Geographic limitations on data storage and processing.
- Right to audit the vendor's security controls, either directly or through a third-party audit report.
- Compliance with applicable privacy regulations (GDPR, CCPA, etc.).

### Cloud Service Provider Requirements

Contracts with cloud service providers must additionally address:

- Shared responsibility model: delineation of security controls managed by the provider vs. the organization.
- How to obtain and utilize security capabilities provided by the cloud service.
- How to obtain assurance on security controls (SOC reports, audit certifications).
- Management of controls across multiple cloud services.
- Incident handling procedures specific to cloud environments.
- Exit strategy: data extraction, migration, and secure deletion upon termination.
- Advance notification of substantive changes affecting the service (infrastructure changes, new jurisdictions for data, use of subcontractors).

### Vendor Access Controls

- Vendor access to organizational systems must follow the principle of least privilege.
- Vendor access must be time-bound and automatically expire when the engagement ends.
- Vendor access must use multi-factor authentication.
- Vendor activity must be logged and monitored.
- Vendors must not access production systems without explicit authorization and justification.

### Vendor Offboarding

When a vendor relationship ends:

- All vendor access must be revoked within ____ business days.
- Organizational data held by the vendor must be returned or certified as securely destroyed.
- The vendor's entry in the inventory must be updated to reflect the terminated relationship.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy. Vendors who fail to meet security requirements may have their access suspended or their contract terminated.

## Related Documents

- Information Security Policy (ISP-001)
- Data Protection Policy
- System Access Control Policy
- Risk Assessment Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
