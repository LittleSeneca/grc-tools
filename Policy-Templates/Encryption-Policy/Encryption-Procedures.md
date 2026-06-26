# Encryption Policy — Implementation Procedures

> **Companion to:** Encryption Policy (Template.md)
> **Purpose:** These procedures describe how to implement the encryption requirements defined in the policy, including key management lifecycle, device encryption deployment, and TLS configuration.

## Procedure: Cryptographic Key Management Lifecycle

### Standard Approach

#### Key Generation

1. **Determine the appropriate key generation method:**
   - **Cloud KMS (preferred):** Use AWS KMS, Azure Key Vault, or GCP Cloud KMS. The HSM-backed key generation ensures FIPS 140-2 Level 2/3 compliance.
   - **On-premises HSM:** For organizations with physical HSM appliances. Slower to provision but necessary for air-gapped environments.
   - **Software-based (least preferred, requires exception):** OpenSSL or similar. Only acceptable for development/non-production keys.
2. **Configure key specifications:**
   - **Symmetric (AES):** 256-bit, generated within the KMS/HSM.
   - **Asymmetric (RSA):** 3072-bit minimum.
   - **Asymmetric (ECC):** NIST P-256 or Ed25519.
   - **Random number source:** The KMS/HSM's internal entropy source, seeded from a hardware random number generator.
3. **Document each key at creation:**
   - Key identifier (ARN, key ID, or alias).
   - Algorithm and key size.
   - Creation date.
   - Intended use (e.g., "S3 bucket encryption for production data").
   - Rotation policy.
   - Authorized principals (who can use and manage the key).

#### Key Storage and Protection

1. **Never store private/secret keys in plaintext.** Use one of:
   - KMS/HSM: keys never leave the hardware boundary. Applications request cryptographic operations via API.
   - Envelope encryption: encrypt the data encryption key (DEK) with a key encryption key (KEK) stored in KMS. Store only the encrypted DEK alongside the data.
2. **Prohibit key embedding in:**
   - Source code (including comments).
   - Configuration files (use secrets references or environment variable injection from a secrets manager).
   - Version control systems (scan repos with tools like `git-secrets`, `truffleHog`, or `detect-secrets`).
   - CI/CD logs (mask secrets in build output).
   - Docker images (use build secrets, not COPY instructions).
3. **Restrict access to key material:**
   - Use IAM policies / access policies scoped to specific principals and specific key operations (encrypt, decrypt, sign, verify).
   - No principal should have `*` permissions on any key.
   - Review key access policies quarterly — remove any overly permissive grants.

#### Key Rotation

1. **Configure automatic rotation where supported:**
   - AWS KMS: enable automatic annual rotation for customer-managed keys.
   - Azure Key Vault: configure key rotation policy with desired interval.
   - GCP Cloud KMS: enable automatic rotation.
   - TLS certificates: use an automated certificate management tool (e.g., cert-manager for Kubernetes, AWS ACM) that renews certificates before expiry.
2. **For systems without automatic rotation** (e.g., application-layer encryption keys, legacy systems), implement a manual rotation procedure:
   - Generate new key in KMS/HSM.
   - Re-encrypt data with the new key. For large datasets, this may need to happen incrementally or during a maintenance window.
   - Mark the old key as "retired" (keep for decryption of old data, prevent new encryption).
   - Retire old key versions after the retention period.
3. **Rotation schedule (from policy):**
   - Data encryption keys (DEKs): every ____ months.
   - Key encryption keys (KEKs): every ____ months.
   - TLS certificates: every ____ months.
   - API keys / service account credentials: every ____ months.
   - Signing keys: every ____ months.

#### Key Revocation and Destruction

1. **Revoke immediately upon suspected compromise:**
   - Disable the key in KMS/HSM (revoke all cryptographic operations).
   - Revoke associated TLS certificates (CRL/OCSP).
   - Rotate all credentials that may have been exposed alongside the key.
2. **Investigate the scope of compromise:**
   - What data was encrypted with this key?
   - Was the key used to sign anything (certificates, code, documents)?
   - Could an attacker have used it to decrypt or sign?
3. **Destroy the key when no longer needed:**
   - KMS: schedule key deletion (minimum waiting period per policy).
   - HSM: use secure deletion function (cryptographic erasure — overwrite key material with zeros or random data).
   - Document the destruction: key ID, date, method, and authorizing individual.
4. **Key destruction is auditable** — log all destruction events to the centralized logging platform and retain logs per retention policy.

### Alternative Approaches

> **💡 Why you might choose differently:** Cloud KMS costs can add up with thousands of keys.

- **Alternative A — HashiCorp Vault:** Use Vault as an abstraction layer over cloud KMS. Provides unified API, audit logging, and dynamic secrets. More operational complexity but better multi-cloud support.
- **Alternative B — BYOK (Bring Your Own Key):** Generate keys in your own HSM and import them into the cloud provider's KMS. You control the key origin, but the cloud provider performs cryptographic operations. Satisfies some regulatory requirements for key sovereignty.
- **Alternative C — Hardware-bound keys:** For highest-security applications, use physical HSMs or smart cards for key storage. Keys NEVER leave the hardware. Highest security but lowest convenience and scalability.

### Common Pitfalls

> **⚠️ Watch out:** "Automatic key rotation" in cloud KMS keeps old key versions available for decryption by default. The rotation is transparent to applications — they reference the key alias, not the version. BUT if you manually delete old key versions, anything encrypted with them becomes permanently unrecoverable.
>
> **⚠️ Watch out:** Keys stored in `.env` files that get committed to Git "just for local development" will be discovered by attackers scraping public repos within minutes. Use `.env.example` with placeholder values and inject real secrets at runtime.
>
> **⚠️ Watch out:** Key rotation that generates a new key but never actually re-encrypts data with it is theater. The old key still decrypts everything, and if it's compromised, rotation didn't help. Rotation must include data re-encryption or a clear plan for when old data ages out.

---

## Procedure: Full-Disk Encryption Deployment

### Standard Approach

#### macOS Devices (FileVault)

1. **Verify MDM enrollment:** The device must be enrolled in the organization's MDM platform (Jamf, Kandji, Intune, etc.).
2. **Enable FileVault via MDM policy:**
   - Deploy a configuration profile that enforces FileVault with the following settings:
     - Enable FileVault: Required.
     - Escrow recovery key to MDM: Required.
     - Show recovery key to user: Optional (recommended: do NOT show — users will screenshot it and save it insecurely).
   - The policy should apply to all managed macOS devices with no opt-out.
3. **Monitor encryption status:**
   - MDM dashboard should report encryption status for every managed device.
   - Configure an alert if any device reports FileVault as disabled or in-progress for more than ____ hours.
4. **Handle encryption failures:**
   - If FileVault fails to enable (e.g., disk errors, unsupported configuration), the IT team must investigate and resolve within ____ business days.
   - Devices that cannot be encrypted must not be used to access organizational data.

#### Windows Devices (BitLocker)

1. **Verify MDM or Active Directory enrollment.**
2. **Enable BitLocker via MDM/GPO policy:**
   - Encryption method: AES-256 (not the default AES-128).
   - Encrypt entire system drive (not just used space — this leaves deleted data unencrypted).
   - Require TPM + PIN for boot (optional; TPM-only is acceptable for most organizations).
   - Escrow recovery key to MDM or Active Directory.
3. **Monitor encryption status:**
   - MDM dashboard or AD query for BitLocker status.
   - Alert on any device without BitLocker enabled.

#### Mobile Devices (iOS / Android)

1. **Enforce MDM enrollment** for all devices accessing organizational data.
2. **Verify encryption is enabled:**
   - iOS: encryption is enabled by default when a passcode is set. MDM policy should require a passcode (minimum length per Password Policy).
   - Android: encryption is enabled by default on modern devices. MDM policy should enforce device encryption and block unencrypted devices.
3. **Prohibit jailbroken/rooted devices:**
   - MDM policy must detect and block jailbroken/rooted devices from accessing organizational data.
   - Configure an alert if a managed device is detected as jailbroken/rooted.

### Alternative Approaches

> **💡 Why you might choose differently:** Not all organizations use MDM, or MDM may not cover all device types.

- **Alternative A — Manual enablement with audit:** For organizations without MDM, IT manually enables encryption during device provisioning and verifies status during annual asset audit. Higher risk of gaps.
- **Alternative B — Self-service with attestation:** Employees self-enable FileVault/BitLocker following provided instructions and attest annually. Only appropriate for very small organizations with high trust.

### Common Pitfalls

> **⚠️ Watch out:** FileVault recovery keys escrowed to MDM are only useful if the MDM is accessible when you need them. If the MDM is cloud-based and the device can't connect to the internet (because the disk is locked), you have a chicken-and-egg problem. Keep a break-glass procedure: how do you boot and connect to network before FileVault unlock?
>
> **⚠️ Watch out:** BitLocker with TPM-only means anyone who can physically access the powered-on device can access the data. TPM + PIN is more secure but adds user friction.
>
> **⚠️ Watch out:** "Encryption is enabled" in the MDM dashboard is a point-in-time check. A user who disables FileVault after provisioning won't be caught until the next MDM sync (which could be hours or days). Configure continuous compliance checks.

---

## Procedure: TLS Configuration and Enforcement

### Standard Approach

#### Server-Side TLS Configuration

1. **Minimum TLS version: 1.3** for all new deployments. TLS 1.2 permitted only for backward compatibility.
2. **Cipher suite configuration (TLS 1.2, where used):**
   - Allow ONLY: `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`, `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`.
   - Explicitly disable: all CBC-mode ciphers, all non-PFS ciphers, all ciphers with <256-bit keys.
3. **Certificate management:**
   - Use ECDSA (NIST P-256) certificates for new deployments.
   - RSA certificates: minimum 3072-bit key.
   - Automate certificate renewal (cert-manager, ACM, Let's Encrypt with auto-renewal).
4. **HSTS (HTTP Strict Transport Security):** Enable on all HTTPS endpoints with `max-age` of at least 1 year and `includeSubDomains`.
5. **Verify configuration:**
   - Use `testssl.sh`, SSL Labs, or `nmap --script ssl-enum-ciphers` to validate the deployed configuration.
   - Integrate TLS testing into CI/CD: scan staging endpoints before production deployment.

#### Client-Side TLS Enforcement

1. **Application-level:** Applications must validate TLS certificates (no `InsecureSkipVerify` or equivalent). Certificate pinning may be used for high-security applications.
2. **Network-level:** Egress firewall rules should require TLS for all outbound connections carrying data classified as Internal or higher.

#### TLS Monitoring

1. **Certificate expiry monitoring:**
   - Monitor all TLS certificates for expiry. Alert at 30 days, escalate at 14 days, critical at 7 days before expiry.
   - Use certificate transparency logs to detect unauthorized certificate issuance for organizational domains.
2. **Protocol/cipher monitoring:**
   - Continuously scan all public-facing endpoints for TLS configuration drift.
   - Alert if a previously TLS 1.3 endpoint drops to TLS 1.0, or if a prohibited cipher suite appears.

### Alternative Approaches

> **💡 Why you might choose differently:** Some internal services have legitimate reasons for not running TLS.

- **Alternative A — Service mesh TLS:** Use a service mesh (Istio, Linkerd, Consul Connect) to provide mutual TLS between services transparently. Services communicate over HTTP; the mesh upgrades to mTLS. Reduces per-service TLS configuration burden.
- **Alternative B — TLS termination at load balancer:** Terminate TLS at the load balancer/API gateway and use HTTP internally within a trusted network. Simpler per-service configuration but data is unencrypted on the internal network.

### Common Pitfalls

> **⚠️ Watch out:** TLS 1.3 with an RSA 2048-bit certificate and an obsolete cipher still isn't secure. TLS version is just one dimension — validate the full configuration (protocol, ciphers, certificate key size, HSTS, certificate validity).
>
> **⚠️ Watch out:** Expired TLS certificates are one of the most common causes of production outages. Automate renewal. If you're running `certbot renew` in a cron job, monitor that the cron job actually ran — cron failures are silent.
>
> **⚠️ Watch out:** "We only use TLS internally" is a common rationalization that falls apart when an attacker gets inside the network. Internal TLS prevents lateral movement after initial compromise.

---

## Procedure: Data-at-Rest Encryption Verification

### Standard Approach

1. **Database encryption verification:**
   - For each database storing Restricted or Confidential data, verify encryption is enabled:
     - **RDS/Aurora:** Check encryption status via AWS Console or CLI (`aws rds describe-db-instances --query 'DBInstances[*].StorageEncrypted'`).
     - **Cloud SQL:** Check encryption configuration.
     - **Self-managed:** Verify TDE or filesystem encryption is enabled.
   - Quarterly audit: export a report of all databases and their encryption status.
2. **Cloud storage encryption verification:**
   - For each S3 bucket / GCS bucket / Azure Blob container storing Restricted or Confidential data:
     - Verify default encryption is configured (server-side encryption with CMK where required).
     - Verify bucket policies deny unencrypted uploads (`aws:s3:x-amz-server-side-encryption` condition).
   - Use CSPM tools for continuous monitoring of encryption configuration.
3. **Backup encryption verification:**
   - Verify that backup jobs output encrypted data (check file headers, or attempt to read without the key — should fail).
   - Include backup encryption verification in quarterly restore tests.
4. **Endpoint encryption verification:**
   - MDM dashboard must report encryption status for all managed devices.
   - Generate monthly compliance report: encrypted devices / total devices.
   - Target: 100% encryption compliance for all devices accessing organizational data.

### Common Pitfalls

> **⚠️ Watch out:** An S3 bucket with "default encryption enabled" still accepts unencrypted uploads if the client explicitly sends an unencrypted PUT. The default only applies when no encryption headers are specified. Use bucket policies to DENY unencrypted uploads.
>
> **⚠️ Watch out:** "Transparent data encryption" in databases encrypts data at rest on disk, but data in memory and in transit is unencrypted. TDE protects against physical disk theft, not application-layer attacks.

## Related Documents

- Encryption Policy (Template.md)
- Data Classification Policy
- Data Protection Policy
- Password Policy
- System Access Control Policy
- Asset Management Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
