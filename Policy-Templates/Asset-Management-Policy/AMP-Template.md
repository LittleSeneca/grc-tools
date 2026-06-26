# Asset Management Policy

Policy Title: Asset Management Policy
Policy Number: ISP-004
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

The purpose of this policy is to define requirements for managing and properly tracking assets owned, managed, and under the control of ____ through their lifecycle from initial acquisition to final disposal.

## Scope

This policy applies to all physical and virtual assets owned, leased, or managed by the organization, including but not limited to servers, workstations, laptops, mobile devices, networking equipment, virtual machines, software licenses, and cloud resources.

## Policy

### Physical and Virtual Asset Standard

The organization must ensure proper management of assets to maximize information security. The following procedures must be enforced:

- A detailed asset inventory must be maintained to track and monitor all significant assets.
- Items may be excluded from the inventory only if they carry very low purchase/replacement costs (including time and labor needed to install and configure) and pose little or no risk to business operations or compliance status.
- Each significant asset must be associated with an identifier, license, or tag, and proper classification when applicable. Details must include a description of the type, make/model, technical specifications, license details, and software versions.
- Assets that contain, store, or handle information must be classified per the Data Classification Policy.
- The disposal or replacement of assets must be tracked, whether due to depreciation, expiring leases, obsolescence, loss, or other reasons.
- A reporting function must support auditing and monitoring for compliance.

### Asset Inventory Standard

An asset inventory process must be in place to support the management of critical business processes and meet legal and regulatory requirements. The inventory process must also support discovery, management, and replacement or disposal of all assets.

All physical and virtual assets under organizational management or control must include:

- Unique identifier (e.g., serial number) or name of the asset
- Description of the asset
- Purpose of the asset and the role it supports in critical business processes and in meeting legal or regulatory requirements
- Entity responsible for the asset
- Classification of the asset, as prescribed in the Data Classification Policy

### Physical Asset Inventory

The organization must maintain an inventory of all organization-owned physical computing equipment, including but not limited to:

- Servers
- Workstations
- Laptops
- Printers
- Networking equipment

All organization-owned devices are subject to a complete data wipe if deemed necessary, such as in the case of device infection or repurposing. This data wipe must be carried out by authorized personnel using approved sanitization methods.

### Digital Asset Inventory

The organization must maintain records of all digital assets, including but not limited to:

- Virtual machines
- Virtual servers (cloud instances)
- Virtual repositories
- Security agents
- Source code repositories
- User accounts

Records must be tagged with owner or project and classification when applicable. All records must be kept up to date through automation where possible.

### Asset Retirement Standard

The ____ determines when an asset is no longer needed or is obsolete. If the asset to be replaced or retired supports mandatory legal or regulatory requirements of critical business processes, the information resource owner must ensure that any replacement asset can support these processes before the current asset is retired.

Before retiring or replacing any asset that retains data, data retention requirements for all data stored or managed by that asset must be reviewed. Any data subject to data retention requirements must be migrated to an appropriate destination and tested for appropriateness, completeness, accessibility, and retrievability before the original data is deleted.

### System Hardening Standards

#### Device Best Practices and Hardening

Manufacturer-provided hardening and best practice guides must be employed to ensure all device installations are properly guarded from vulnerabilities.

- Center for Internet Security (CIS) benchmarks or equivalent industry standards should be utilized for system hardening guidance.
- Vendor-supplied defaults, including usernames, passwords, and common settings, must be changed in accordance with hardening guides.
- Insecure and unnecessary communication protocols must be disabled.
- Local passwords, when required, must be randomly generated and securely stored in the approved password management system.
- Current patches must be installed and an automatic, ongoing patch management process must be maintained.
- Malware protection must be implemented.
- Logging must be enabled.
- Multi-factor authentication must be used whenever available and supported on the device platform.

#### Infrastructure Configuration and Maintenance

**Internal Workstation and Server Patching:**

- Operating system patches and upgrades must be evaluated periodically.
- Patches and upgrades must be installed based on criticality.
- Patches and upgrades must be installed during off-peak hours to minimize disruption.

**Internal Infrastructure Patching:**

- Infrastructure patches and upgrades must be evaluated as they become available from vendors.
- Patches and upgrades must be installed based on criticality.
- Patches and upgrades must be reviewed and approved via a lab or staging environment when possible.
- When applicable, redundant systems must be patched or upgraded one device at a time to ensure no impact to shared services.

**Infrastructure Support Documentation:**

- A network diagram must be available to all appropriate service personnel and kept current.
- Configuration standards for the setup of all infrastructure devices must be in place and formally documented.

**Endpoint Security:**

- Anti-malware tools must be deployed on endpoint devices.
- Anti-malware tools must be configured to automatically receive updates, run scans, and alert appropriate personnel.

### Physical Media Transfer

Secure electronic transfer should be used whenever possible. When physical media must be transported:

- Data on physical media must be encrypted using approved encryption methods.
- Reliable transport or couriers must be used, with a management-approved list and identification verification procedures.
- Packaging must be sufficient to protect contents from physical damage during transport.
- Transfers must be logged with information about content, protection applied, time of transfer to transport custodian, and time of receipt at destination.

### Return of Assets Upon Termination

- The termination process must include the return of all previously issued physical and electronic assets owned by or entrusted to the organization.
- If organization equipment was purchased by an employee or third party, or personal equipment was used, all relevant information must be transferred to the organization and securely erased from the equipment.
- Unauthorized copying of information during the termination period must be monitored and controlled.

### Disposal of Media

The steps for secure disposal of media containing confidential information must be proportional to the sensitivity of that information:

- Identification of items requiring disposal.
- Use of appropriate third-party collection and disposal services in accordance with the Vendor Management Policy.
- Secure disposal by incineration or shredding, or erasure of data for reuse within the organization.
- Risk assessment of damaged media to determine disposal or repair.
- Full-disk encryption to mitigate risk of disclosure of confidential information.
- Logging each disposal to maintain an audit trail.

### Media Sanitization

Media sanitization procedures must ensure that when data is removed, it cannot be retrieved or reconstructed:

- Media must be securely wiped before disposal or when the information is no longer needed.
- Sanitization procedures and equipment must be reviewed and tested annually to ensure intended sanitization is being achieved.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ | Inventory maintenance; asset lifecycle management |
| Information Resource Owners | Data migration; retirement decisions |
| ____ | Sanitization; disposal oversight |

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- Data Classification Policy
- Encryption Policy
- Vendor Management Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
