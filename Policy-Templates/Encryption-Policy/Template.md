# Encryption Policy

Policy Title: Encryption Policy
Policy Number: ISP-005
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy defines organizational requirements for the use of cryptographic controls and cryptographic keys to protect the confidentiality, integrity, authenticity, and non-repudiation of information.

## Scope

This policy applies to all systems, equipment, facilities, and information within the scope of ____'s Information Security Program. It applies to all Personnel involved with cryptographic systems, algorithms, or keying material.

## Background

A standardized approach to cryptographic controls ensures end-to-end security and promotes interoperability. This policy defines approved cryptographic algorithms, key management requirements, and requirements for cryptography in cloud environments.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Annual review; policy updates |
| ____ (Infrastructure/Engineering) | Implementation of cryptographic controls |
| All Personnel | Compliance with encryption requirements for their devices and data |

## Policy

### Approved Cryptographic Standards

The following cryptographic standards are approved for use:

| Use Case | Algorithm | Minimum Key Size |
|----------|-----------|-----------------|
| Symmetric Encryption (data at rest) | AES-256-GCM | 256 bits |
| Symmetric Encryption (data in transit) | AES-256-GCM or ChaCha20-Poly1305 | 256 bits |
| Asymmetric Encryption | RSA-4096 or ECDSA/EdDSA (Curve25519) | 4096 bits / 256 bits |
| Hashing | SHA-256 or SHA-384 | N/A |
| Key Exchange | ECDHE with Curve25519 or P-384 | 256 bits / 384 bits |
| TLS | TLS 1.3 (TLS 1.2 acceptable only where 1.3 is unavailable) | Per TLS specification |

The following are explicitly **prohibited**:

- DES, 3DES, RC4, MD5, SHA-1 (for security purposes)
- TLS 1.0, TLS 1.1, SSLv2, SSLv3
- AES in ECB mode
- Custom or proprietary cryptographic algorithms

### Data at Rest Encryption

- All databases, data stores, and file systems containing sensitive or customer data must be encrypted at rest.
- Full-disk encryption must be enabled on all employee workstations and laptops:
  - **macOS:** FileVault with institutional key recovery
  - **Windows:** BitLocker with TPM and recovery key escrow
  - **Linux:** LUKS with key escrow
- Cloud storage volumes must use provider-managed or customer-managed encryption keys.
- Encryption status must be monitored, and unencrypted volumes must generate alerts.

### Data in Transit Encryption

- All external data transmissions must be encrypted end-to-end using TLS 1.3 (minimum TLS 1.2).
- Internal network traffic should be encrypted where feasible, particularly for sensitive data.
- VPN connections must use WireGuard or IPsec with approved algorithms.
- Email containing sensitive data must be transmitted using TLS-encrypted channels or end-to-end encryption.

### Key Management

- Cryptographic keys must be managed through an approved key management service (KMS) with the following capabilities:
  - **Key Access:** Designated users can encrypt/decrypt and generate data encryption keys.
  - **Key Administration:** Designated users can create, schedule deletion, enable/disable rotation, and set usage policies.
  - **Backup and Storage:** Keys must be securely stored and backed up for their operational lifetime.
  - **Rotation:** Keys must be rotated at least once every ____ months (recommended: 12 months). Service-specific keys (TLS certificates) may require shorter rotation periods.
- Keys must be protected against loss, change, or destruction using appropriate access controls.
- Private keys must never be transmitted in cleartext or stored in source code repositories.
- Key access must be logged and monitored.

### Employee Device Encryption

- All employee computers must use full-disk encryption with institutional key recovery.
- Employees must verify encryption is enabled on their devices.
- Recovery keys must be escrowed with the IT department or MDM platform.
- Lost devices with unknown encryption status must be treated as potential data breaches.

### Cloud Service Provider Encryption

When using cloud services, the organization must:

- Verify that the provider's default encryption meets or exceeds this policy's standards.
- Consider customer-managed keys (CMK) for sensitive workloads.
- Review provider encryption practices during vendor assessments.

### Governing Law

Encryption practices must comply with all applicable local, regional, and international laws and regulations, including import/export controls on cryptographic technology.

## Compliance and Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- Data Protection Policy
- Data Classification Policy
- Vendor Management Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
