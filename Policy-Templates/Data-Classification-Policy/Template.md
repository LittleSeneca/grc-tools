# Data Classification Policy

Policy Title: Data Classification Policy
Policy Number: ISP-010
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes the data classification scheme for ____ and defines handling requirements for each classification level to ensure data is protected according to its sensitivity and business impact.

## Scope

This policy applies to all information owned, managed, controlled, or maintained by ____, regardless of form or media. This includes electronic, hardcopy, and verbal communications.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Annual review; classification guidance |
| Data Owners | Classify data under their ownership |
| All Personnel | Handle data according to its classification |

## Policy

### Data Classification Scheme

All organizational data must be classified into one of the following four levels:

| Classification | Definition | Examples |
|----------------|-----------|----------|
| **Restricted** | Data whose unauthorized disclosure, alteration, or destruction would cause **serious or critical** damage to the organization, its customers, or partners. Typically protected by law, regulation, or contract. | Customer PII, authentication credentials, encryption keys, financial account numbers, protected health information (PHI) |
| **Confidential** | Data whose unauthorized disclosure, alteration, or destruction would cause **significant** damage. Includes proprietary business information and sensitive internal data. | Source code, business strategy, security architecture, contracts, employee records, internal financials |
| **Internal** | Data intended for internal use. Unauthorized disclosure would cause **moderate** or minimal damage. This is the default classification for all data not otherwise classified. | Internal emails, project documentation, meeting notes, non-sensitive operational data |
| **Public** | Data approved for public release. Unauthorized disclosure would cause **no damage**. | Marketing materials, public website content, published research, job postings |

### Classification Assignment

- Data must be classified at the time of creation or acquisition.
- The data owner is responsible for assigning and periodically reviewing the classification.
- If classification is unclear, the higher (more restrictive) classification must be applied until a determination is made.
- Data that combines multiple classification levels must be protected at the highest level represented.

### Handling Controls by Classification

| Control | Restricted | Confidential | Internal | Public |
|---------|-----------|-------------|----------|--------|
| Access Control | Need-to-know; explicit authorization required | Role-based access; business need | Role-based access | No restrictions |
| Encryption at Rest | Required | Required | Recommended | Not required |
| Encryption in Transit | Required | Required | Recommended | Not required |
| External Sharing | Prohibited without NDA and encryption | NDA required; encryption required | NDA recommended | No restrictions |
| Mobile Device Storage | Prohibited without encryption and remote wipe | Encryption required; remote wipe recommended | Encryption recommended | No restrictions |
| Printing | Prohibited unless necessary; secure print release required | Allowed; secure disposal required | Allowed; disposal recommended | No restrictions |
| Disposal | Secure destruction (shredding, cryptographic erasure) | Secure destruction | Standard deletion | No special requirements |
| Email (external) | Prohibited without end-to-end encryption | Encryption required | Encryption recommended | No restrictions |

### De-identification

When data is de-identified or anonymized, all direct and indirect identifiers must be removed such that re-identification is not reasonably possible. If a dataset contains any personal information after processing, it must not be considered de-identified and remains subject to its original classification.

### Reclassification

- Data classification must be reviewed when the data's use, sensitivity, or regulatory context changes.
- The data owner may reclassify data at any time. Downgrading classification requires documented justification.
- At minimum, data classifications must be reviewed annually.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- Data Protection Policy
- Data Retention Policy
- Encryption Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
