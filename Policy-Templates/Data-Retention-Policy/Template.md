# Data Retention Policy

Policy Title: Data Retention Policy
Policy Number: ISP-012
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy defines the requirements and procedures for retaining and deleting data throughout its lifecycle, ensuring compliance with legal, regulatory, contractual, and business requirements.

## Scope

This policy applies to all data owned, managed, or processed by ____, regardless of form or location, including customer data, employee data, financial records, and operational data.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer / Privacy Officer) | Annual review; retention schedule maintenance |
| Data Owners | Apply retention periods to data under their ownership |
| ____ (Legal/Compliance) | Regulatory retention requirements |
| ____ (IT/Engineering) | Technical implementation of retention and deletion |

## Policy

### Retention Principles

- Data must only be retained for as long as necessary to fulfill its business purpose and meet legal or regulatory requirements.
- Data retention periods must be defined and documented for each category of data.
- At the end of the retention period, data must be securely deleted unless a legal hold is in place.
- Data subject to litigation, investigation, or audit must be preserved until the legal hold is released.

### Retention Schedule

| Data Category | Retention Period | Rationale |
|---------------|-----------------|-----------|
| Customer Data (active accounts) | Duration of account activity + ____ days after closure | Contractual and business requirements |
| Customer Data (inactive/suspended accounts) | ____ days from suspension | Grace period for account reinstatement |
| Employee Records | Duration of employment + ____ years | Legal and HR requirements |
| Financial Records | ____ years | Tax and regulatory requirements |
| Security Logs and Audit Trails | ____ years (minimum 1 year recommended) | Security investigation and compliance |
| System Backups | ____ days | Operational recovery; older data available in archives |
| Operational Data (metrics, reports) | ____ years | Business needs |
| Marketing and Analytics Data | ____ years or until consent withdrawal | Privacy regulations |
| Contracts and Legal Documents | ____ years after termination | Legal requirements |

### Data Deletion

When data reaches the end of its retention period and no legal hold applies:

- Data must be securely deleted using approved methods (cryptographic erasure, secure overwrite per NIST SP 800-88, or physical destruction).
- A record of deletion must be maintained, including: data category, deletion date, method used, and authorizing party.
- Cloud provider data deletion capabilities may be used where they meet the secure deletion standard.

### Legal Holds

- When a legal hold is issued, all data relevant to the hold must be preserved immediately, overriding normal retention schedules.
- The ____ (Legal/Compliance) team is responsible for issuing and releasing legal holds.
- A record of all active legal holds must be maintained.
- Data subject to legal hold must not be modified or deleted until the hold is released.

### Backups and Archives

- Data in backups is subject to the same retention and deletion requirements as live data.
- When data is deleted from production systems, corresponding backup data must be deleted within the next backup rotation cycle.
- Archived data that has reached end-of-retention must be securely destroyed.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy, and may have legal or regulatory consequences.

## Related Documents

- Information Security Policy (ISP-001)
- Data Classification Policy
- Data Protection Policy
- Backup Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
