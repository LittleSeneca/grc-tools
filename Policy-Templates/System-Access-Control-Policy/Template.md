# System Access Control Policy

Policy Title: System Access Control Policy
Policy Number: SAC-001
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes the requirements and procedures for provisioning, modifying, reviewing, and revoking access to ____'s information systems and data. It ensures that access is granted based on the principle of least privilege, properly authorized, and regularly reviewed to maintain alignment with job responsibilities.

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

#### New Account Creation

1. The requester submits an access request through the approved ticketing or identity management system.
2. The request must specify: individual's name and role, systems requested, level of access, and business justification.
3. The request must be approved by the system owner or designated approver.
4. Identity verification must be performed (in-person preferred; for remote, out-of-band verification required).
5. Access is provisioned based on role and minimum necessary permissions.
6. The individual must acknowledge applicable policies before access is activated.

#### Role-Based Access Control (RBAC)

- Roles must be defined based on job functions, not individual identities.
- Each role must be documented with access description and business justification.
- Role membership must be reviewed when job responsibilities change.
- Custom permission grants should be the exception; repeated patterns should become defined roles.

#### Privileged Access

- Must be explicitly authorized by the system owner and Security Officer.
- Just-in-time (JIT) provisioning with automatic expiration preferred (not to exceed ____ hours, e.g., 8).
- All privileged access use must be logged and attributable to the individual.
- Privileged access grants must be reviewed at least ____ (e.g., monthly).

#### Third-Party and Contractor Access

- Must be limited in scope and duration with a defined expiration date.
- A valid contract with security and confidentiality obligations must be in place.
- Third-party access must be reviewed at least ____ (e.g., monthly).
- Access must be promptly revoked upon contract completion or termination.

### Access Reviews

1. Initiated by the Security Officer on a ____ (e.g., quarterly) schedule.
2. System owners and managers review access rights of users within their purview.
3. Review confirms: individual still requires access, level of access is appropriate, no unauthorized escalation.
4. Unneeded access must be modified or revoked; discrepancies must be remediated and documented.
5. Review must be completed within ____ (e.g., 30) days of initiation.
6. Results and changes must be documented and retained as evidence.

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

#### Voluntary Departure
- HR notifies Security Officer/IT of departure date.
- Physical access revoked by end of last working day.
- Logical access revoked within ____ (e.g., 1) business day.
- Termination checklist documents all access removal and asset return.

#### Involuntary Termination
- Access must be revoked immediately upon notification — before or simultaneously with notification to the individual.

#### Role Change
- Previous role access must be reviewed and removed if no longer needed.
- New role access provisioned through standard process.
- Access should not accumulate across roles.

#### Extended Inactivity
- Accounts inactive for ____ (e.g., 90) days must be disabled.
- Disabled accounts unused for an additional ____ (e.g., 90) days must be deleted.

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

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
