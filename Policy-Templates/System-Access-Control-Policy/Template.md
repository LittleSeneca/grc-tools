# System Access Control Policy

Policy Title: System Access Control Policy
Policy Number: ISP-013
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes the requirements for creating, modifying, reviewing, and removing access to ____ systems, applications, and data. Access is granted on a least-privilege, need-to-know basis.

## Scope

This policy applies to all Personnel who require access to organizational systems and data. It governs the full access lifecycle: provisioning, modification, review, and revocation.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Access reviews; policy enforcement |
| ____ (IT/Infrastructure) | Technical access provisioning and revocation |
| Managers | Approve access requests; verify ongoing need |
| All Personnel | Report unauthorized access or access anomalies |

## Policy

### Access Control Principles

- **Least Privilege:** Access must be limited to the minimum necessary to perform job functions.
- **Need to Know:** Access to sensitive data must be granted only when there is a legitimate business need.
- **Separation of Duties:** Critical functions must be divided among multiple individuals to prevent fraud or error.
- **Role-Based Access Control (RBAC):** Access must be assigned based on job role, not individual preference.

### Access Provisioning

Requests for access must follow a formal process:

1. The requester or their manager submits an access request through the designated system (e.g., ticketing system, Identity Provider workflow).
2. The request must specify: user identity, systems or data to access, level of access needed, and business justification.
3. User identity must be verified before granting access. For remote provisioning, identity verification must occur via an approved secondary channel (video call, manager confirmation).
4. The access approver (manager or system owner) reviews and approves or denies the request.
5. Approved access is provisioned within ____ business days.
6. The provisioning action must be logged with user identity, access granted, approver, and timestamp.

### Access Reviews

All user access must be reviewed regularly:

- **Frequency:** Quarterly for privileged access; semi-annually for standard access.
- **Process:** The ____ (Security Officer / IT) initiates the review. Managers confirm that each team member's access remains appropriate for their current role.
- **Remediation:** Access no longer needed must be revoked within ____ business days of the review.
- **Documentation:** Review results must be documented, including any access modifications made.

### Privileged Access Management

Privileged accounts (administrator, root, database admin) are subject to additional controls:

- Privileged access must be granted only when necessary for the role.
- Shared privileged accounts are prohibited. Each administrator must use an individually named account.
- Privileged access must use multi-factor authentication.
- Privileged session activity must be logged and monitored.
- Just-in-time (JIT) access should be used where possible: elevated privileges are granted for a specific task and limited time window, then automatically revoked.
- Privileged access must be reviewed quarterly at minimum.

### Unique User Identification

- Each user must have a unique account. Shared accounts are prohibited.
- Default, guest, and vendor accounts must be disabled when not in use.
- Service accounts must be documented, have a designated owner, and use the minimum privileges required.
- Accounts must not be shared between environments (development accounts must not have production access).

### Automatic Logoff

- Sessions must automatically terminate after ____ minutes of inactivity.
- Re-authentication must be required to resume the session.

### Employee Termination and Access Revocation

- The ____ (HR) department must notify ____ (IT/Security) of pending terminations at least ____ days in advance where possible.
- All access must be revoked within ____ business day of termination.
- For involuntary terminations, access must be revoked immediately upon notification.
- Access for employees on extended leave may be temporarily suspended.
- Accounts inactive for ____ days must be automatically disabled.
- The termination process must include verification that all access has been revoked.

### Workstation Standards

- All workstations must be organization-managed and meet security baseline requirements.
- Workstations must not be used for illegal activities or activities violating organizational policies.
- Workstation hard drives must be encrypted.
- Host-based firewalls must be enabled.
- Local administrative access must be restricted and justified.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- Password Policy
- Acceptable Use Policy
- Data Classification Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
