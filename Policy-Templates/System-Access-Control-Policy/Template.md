# System Access Control Policy

Policy Title: System Access Control Policy
Policy Number: SAC-001
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes the requirements for provisioning, modifying, reviewing, and revoking access to ____'s information systems and data. It ensures that access is granted based on the principle of least privilege, properly authorized, and regularly reviewed to maintain alignment with job responsibilities.

## Scope

This policy applies to all systems, applications, networks, and data repositories owned or managed by ____. It governs access for all users, including employees, contractors, temporary workers, business partners, and any other individuals granted access to organizational information systems.

## Policy

### General Access Principles

Access to organizational systems and data is governed by the following principles:

- **Least Privilege:** Access is limited to the minimum necessary to perform assigned job responsibilities.
- **Need to Know:** Access to sensitive information requires a demonstrable business need.
- **Separation of Duties:** Conflicting responsibilities must be assigned to different individuals where feasible.
- **Individual Accountability:** All access must be attributable to a unique individual. Shared accounts are prohibited except in narrowly defined circumstances with documented justification and compensating controls.
- **Periodic Review:** All access rights must be formally reviewed at least ____ (e.g., quarterly).

### Access Provisioning

Access must be provisioned through a formal, documented process that includes: request submission with business justification, approval by the system owner or designated approver, identity verification, role-based access assignment per the principle of least privilege, and policy acknowledgment before activation. Roles must be defined by job function, not individual identity, and documented. Privileged access must be explicitly authorized, use just-in-time (JIT) provisioning with automatic expiration (max ____ hours), and be logged. Third-party and contractor access must be limited in scope and duration with a defined expiration, supported by a valid contract, and promptly revoked upon contract completion.

> **Implementation procedures:** See [System Access Control Procedures — Procedure 1: Access Provisioning](./System-Access-Control-Procedures.md)

### Access Reviews

Formal access reviews must be conducted by system owners and managers on a ____ (e.g., quarterly) schedule. Each review must confirm that the individual still requires access, the level of access is appropriate, and no unauthorized escalation has occurred. Unneeded access must be revoked; discrepancies must be remediated and documented. Reviews must be completed within ____ (e.g., 30) days of initiation, and results must be documented and retained.

Detailed access review execution workflows and continuous certification procedures are defined in [System Access Control Implementation Procedures](./System-Access-Control-Procedures.md).

### Unique User Identification

- Every user must have a unique user ID. Shared or generic accounts are prohibited except as noted below.
- User IDs must not be reused or reassigned.
- Identifiers should distinguish user types (e.g., employee vs. contractor naming convention).
- Automated logon configurations that bypass password entry are prohibited, except for the approved password manager.

#### Shared Account Exceptions

May be permitted only when: the system does not support individual accounts (legacy), for break-glass emergency access, or other compelling technical constraints. Any shared account exception must be documented, approved by the Security Officer, have its password stored in the password manager with access logging, reviewed at least ____ (e.g., monthly), and eliminated through system replacement as soon as feasible.

### Workstation and Endpoint Access Controls

- Workstations must auto-lock after ____ (e.g., 10) minutes of inactivity.
- Personnel must manually lock workstations when leaving them unattended.
- Workstation firewalls must be enabled.
- Full-disk encryption must be enabled per Encryption Policy.
- Local administrator access must be restricted; standard users must not have administrative privileges by default.

### Offboarding and Access Revocation

Access must be revoked according to the individual's departure type:
Access must be revoked according to the individual's departure type:
- **Voluntary Departure:** Physical access revoked by end of last working day. Logical access revoked within ____ (e.g., 1) business day. A termination checklist documents all access removal and asset return.
- **Involuntary Termination:** Access revoked immediately upon notification — before or simultaneously with notification to the individual.
- **Role Change:** Previous role access must be removed if no longer needed. New role access provisioned through standard process. Access must not accumulate across roles.
- **Extended Inactivity:** Accounts inactive for ____ (e.g., 90) days must be disabled. Disabled accounts unused for an additional ____ (e.g., 90) days must be deleted.

Detailed offboarding and access revocation procedures are defined in [System Access Control Implementation Procedures](./System-Access-Control-Procedures.md).

### Access Control for Customer Data

- Access to customer production data is prohibited by default for all Personnel.
- Temporary access granted only for specific operational needs with explicit approval.
- All customer data access must be logged (who, what, when, why).
- Customer data access logs must be reviewed weekly for anomalies.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (e.g., CISO / Security Officer) | Policy owner; annual review; conduct or delegate access reviews; approve privileged access |
| System Owners | Approve access requests; participate in access reviews |
| Managers | Request appropriate access for team members; verify access during reviews; notify IT of departures |
| IT / IAM | Provision and revoke access; maintain identity management system; enforce technical controls |
| Human Resources | Notify IT of new hires, role changes, and terminations |
| All Personnel | Request only necessary access; report unauthorized access; lock workstations; comply with access policies |

## Compliance and Enforcement

Compliance is verified through quarterly access reviews, automated monitoring of privileged account usage, audit of access logs, and periodic reconciliation of active accounts against active HR records. Violations may result in disciplinary action, up to and including termination.

## Related Documents

- Password Policy
- Data Classification Policy
- Acceptable Use Policy
- Vendor Management Policy
- Encryption Policy
- [System Access Control Implementation Procedures](./System-Access-Control-Procedures.md)

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
