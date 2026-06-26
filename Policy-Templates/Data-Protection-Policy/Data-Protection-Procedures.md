# Data Protection Policy — Implementation Procedures

> **Companion to:** Data Protection Policy (Template.md)
> **Purpose:** These procedures describe how to implement the requirements set forth in the Data Protection Policy. The policy defines WHAT must be done; this document describes HOW to do it.

---

## Procedure 1: Customer Data Segmentation and Protection

### Standard Approach

1. **Data Discovery and Classification**
   - Inventory all repositories that store customer data: databases, file stores, object storage, backups, logs.
   - Tag each repository with the data classification (Restricted/Confidential/Internal) and the owning customer/tenant.
   - Use automated data discovery tools (`____`, e.g., Amazon Macie, Microsoft Purview, Varonis, BigID) to scan for misclassified or shadow customer data stores.

2. **Logical Segmentation Implementation**
   - **Application-Level Enforcement:** Every data access request must include the tenant/customer context. The application layer validates that the requesting user belongs to that tenant before returning data. NEVER rely solely on query parameter filtering.
   - **Database-Level Segmentation Options:**
     - **Database-per-tenant:** Each customer has a dedicated database instance. Strongest isolation, highest operational overhead. Best for <100 tenants or high-compliance environments.
     - **Schema-per-tenant:** Each customer has a dedicated database schema within a shared instance. Moderate isolation, good for 100-1000 tenants.
     - **Row-level with Row-Level Security (RLS):** All tenants share tables, but RLS policies enforce that `customer_id = current_user_customer_id()`. Lowest operational overhead, requires rigorous testing. Best for >1000 tenants.
   - Document the chosen segmentation strategy with rationale in the system architecture document.

3. **Segmentation Validation and Testing**
   - Implement automated integration tests that attempt cross-tenant data access and verify it is blocked.
   - Include "tenant isolation" as a test case in every release's test plan.
   - Conduct manual penetration testing of tenant isolation at least annually and after major architecture changes.
   - Monitor for cross-tenant access anomalies: if an access log shows User-A from Tenant-1 reading Tenant-2's data, generate a Critical alert.

4. **Encryption at Rest for Customer Data**
   - All volumes/databases containing customer data must be encrypted using the organization's key management infrastructure (`____`, e.g., AWS KMS, Azure Key Vault, HashiCorp Vault).
   - Encryption keys must be dedicated per data store, not shared across all customer data repositories.
   - Key access must be restricted to the minimum set of service accounts. No human should have routine access to data encryption keys.
   - Key rotation must be automated on a `____`-day cycle.

5. **Access Logging for Customer Data**
   - Enable database audit logging at the query level for all customer data repositories.
   - Log every access: timestamp, user identity, data accessed (table/object), action (SELECT/INSERT/UPDATE/DELETE), source IP, and session identifier.
   - Forward logs to the centralized SIEM (`____`) in real-time.
   - Configure automated detection rules for: access outside business hours, bulk data retrieval, first-time access to sensitive tables, access from unusual geographic locations.

### Alternative Approaches

> **💡 Why you might choose differently:** The right segmentation strategy depends on your tenant count, compliance requirements, and operational maturity.

- **Customer-Managed Encryption Keys (CMEK):** Offer customers the ability to provide and manage their own encryption keys. The organization cannot access customer data without the customer's key. Strongest customer data protection, but adds operational complexity and requires robust key management UX for customers.
- **Data Localization / Sovereignty:** For GDPR or regional compliance, deploy regional data stores where customer data for EU tenants resides only in EU data centers, US tenant data in US data centers, etc. Use cloud provider region constraints to enforce.
- **Crypto-Shredding for Offboarding:** On customer offboarding, instead of hunting through backups and replicas to delete data, destroy the customer-specific encryption key. All data becomes irrecoverable. Requires per-customer key architecture.

### Common Pitfalls

> **⚠️ Watch out:** Row-level security that can be bypassed by a developer with database admin access. RLS is effective against application users, but a DBA with `BYPASSRLS` privilege or direct table access can read all rows. Defense in depth: encrypt per-tenant, restrict DBA access, audit privileged queries.

> **⚠️ Watch out:** Forgetting about backups, replicas, data warehouse copies, and analytics pipelines. Production data is segmented, but the nightly backup dump to the data lake isn't. Every copy of customer data must inherit segmentation controls.

> **⚠️ Watch out:** Logging customer data itself in access logs. A SQL query log that includes `INSERT INTO patients VALUES ('John Doe', 'HIV-positive', ...)` has now duplicated protected data into logs. Configure audit logging to log query structure, not parameter values containing customer data.

---

## Procedure 2: Production Data Access Controls

### Standard Approach

1. **Default-Deny Posture**
   - By default, NO personnel have access to production environments or production data.
   - This is enforced technically: production IAM roles/security groups grant zero standing access to human users.
   - DevOps/engineering personnel work in development and staging environments with synthetic or anonymized data.

2. **Access Request Workflow**
   1. Personnel submits access request via `____` (ticketing system / IGA platform) with: business justification, specific system/data to be accessed, required access level (read-only, read-write, admin), and requested duration.
   2. Request is routed to the system owner for approval. For customer data access, an additional approval from `____` (Security Officer / Privacy Officer) is required.
   3. Approved requests with duration >`____` hours require a second approver (separation of duties).
   4. Approval must be documented with timestamp and approver identity.

3. **Just-In-Time (JIT) Access Provisioning**
   - Approved access is provisioned via the JIT access broker (`____`, e.g., AWS IAM Identity Center, Azure PIM, Teleport, StrongDM).
   - Access is granted for the EXACT duration approved — not indefinite.
   - At expiration, access is automatically revoked. No manual cleanup required.
   - Personnel receive a notification `____` minutes before expiration; access can be extended only via a new request.
   - All JIT sessions are recorded (terminal session recording, query logging) for audit.

4. **Emergency Access ("Break Glass")**
   - For incident response or critical outages, a streamlined emergency access process bypasses the standard approval workflow.
   - The responder uses a dedicated break-glass account (separate from their daily account).
   - Break-glass account credentials are stored in the password manager with access logging and alerting on checkout.
   - Every break-glass access generates an immediate Critical alert to the Security team.
   - All break-glass sessions are reviewed within `____` hours of the incident resolution.

5. **Access Review and Audit**
   - Production access logs are reviewed `____` (e.g., weekly) for anomalies.
   - A quarterly access review confirms that all standing access and recent temporary access was appropriate and justified.
   - Any unauthorized or unjustified access is escalated to the Security Officer for investigation.

### Alternative Approaches

> **💡 Why you might choose differently:** Organizational size, cloud maturity, and tooling availability influence the feasibility of full JIT.

- **Bastion Host / Jump Server Model:** All production access goes through a hardened bastion host with session recording, command filtering, and proxy authentication. Simpler than full JIT but provides session-level audit trail. Suitable for smaller environments.
- **Zero Standing Access / Dynamic Access:** All access is ephemeral — even engineers have no standing read access to production logs. Every access requires a time-bound, approved, and audited session. Highest security, highest friction. Typically used in highly regulated environments (PCI Level 1, FedRAMP High).
- **Read-Only Replicas for Analysis:** Instead of granting production access for troubleshooting, maintain near-real-time read replicas of production databases. Engineers query replicas with PII masked. Eliminates most routine production access needs.

### Common Pitfalls

> **⚠️ Watch out:** "Temporary" access that never expires. JIT is only as good as the revocation mechanism. If expiration doesn't work reliably — due to misconfiguration, API failure, or manual override — you have permanent access with a temporary label. Test auto-revocation monthly.

> **⚠️ Watch out:** Emergency access normalization. If the break-glass process is used weekly for "emergencies," it's not an emergency process — it's a workaround for a broken standard process. Track break-glass frequency and investigate if it exceeds `____` times per month.

> **⚠️ Watch out:** Shared production credentials. If all engineers share a `readonly-prod` account, you lose individual accountability. Every production access must be attributable to a unique individual, even if the underlying database user is shared — use an access proxy or PAM tool that maps sessions to individuals.

---

## Procedure 3: Data Deletion and Secure Disposal

### Standard Approach

#### 3.1 Deletion Trigger and Authorization

1. Data deletion can be triggered by: retention period expiration (per Data Retention Policy), customer offboarding request, data subject access request (DSAR) under privacy regulations, or identification of orphaned/rogue data stores.
2. Before deletion, the data owner confirms: all legal/regulatory retention obligations are met, data is not subject to litigation hold, and business value has been assessed and exhausted.
3. Deletion authorization is recorded: what is being deleted, by whom, when, and under what authority (policy reference or customer request ID).

#### 3.2 Deletion Methods by Classification

| Classification | Deletion Method | Verification |
|---------------|-----------------|--------------|
| **Restricted** | Cryptographic erasure (destroy key), OR verified multi-pass overwrite per NIST SP 800-88 Purge | Verify key destruction or overwrite completion; retain certificate of sanitization |
| **Confidential** | Cryptographic erasure OR secure deletion utility (single-pass overwrite) | Verify deletion completion; retain log entry |
| **Internal** | Standard file system delete, database DELETE, or S3 object delete | No special verification |
| **Public** | Standard deletion | No special verification |

#### 3.3 Cloud Data Deletion

1. **Object Storage (S3, Blob, GCS):** Enable versioning and MFA Delete on sensitive buckets. Deletion involves deleting all object versions and delete markers. Enable bucket lifecycle policies that enforce permanent deletion after `____` days of soft-delete.
2. **Block Storage (EBS, Managed Disks, Persistent Disks):** Before detaching/deleting a volume, verify that the volume was encrypted with a customer-managed key. Destroy the key first, then delete the volume.
3. **Database (RDS, Cloud SQL, Cosmos DB):** Enable automated backups with encryption. On deletion, ensure the final snapshot (if taken) is encrypted. Delete all manual and automated snapshots. If using customer-managed keys, destroy the key to render all backups unrecoverable.
4. **SaaS Data:** For data stored in SaaS applications (Salesforce, Jira, Slack), follow the vendor's data deletion procedures and obtain written confirmation of deletion. For high-sensitivity environments, contractually require the vendor to provide a certificate of deletion.

#### 3.4 Backups and Replicas

1. Data deletion in the primary store must cascade to backups — but with awareness that backup deletion may take `____` days/weeks depending on retention.
2. Maintain a "deletion tracking" log that records: primary deletion date, expected date by which all backups containing this data will have cycled out, and confirmation date when backup deletion is complete.
3. For customer offboarding, provide the customer with a deletion timeline: "Data will be removed from active systems within `____` days and from backups within `____` days."

#### 3.5 Deletion Verification

1. For Restricted data, after deletion: attempt to access/recover the data using the same method that was used before deletion. Document the "access denied" or "not found" result.
2. Retain deletion records (what, when, how, verified by) for `____` years for audit purposes.
3. Include data deletion in the annual audit cycle: select a random sample of completed deletions and verify that data is irrecoverable.

### Alternative Approaches

> **💡 Why you might choose differently:** Deletion strategy should align with your encryption architecture, backup strategy, and regulatory environment.

- **Crypto-Shredding Only:** Encrypt every data object with a unique or per-tenant key at creation. Deletion = destroy the key. No need to track down copies in backups — they become irrecoverable garbage. Requires rigorous key management but simplifies deletion dramatically.
- **Soft-Delete with Hard-Delete Automation:** Implement a "trash bin" where deleted data is retained for `____` days (recoverable by authorized personnel). After the soft-delete period, automated processes permanently hard-delete. Balances recovery needs with eventual secure deletion.
- **Third-Party Deletion Service:** For highly regulated environments, engage a third-party auditor to witness and certify deletions of high-sensitivity data. This provides independent attestation for compliance.

### Common Pitfalls

> **⚠️ Watch out:** Deletion that only removes the pointer, not the data. A SQL `DELETE` marks rows as free space but doesn't overwrite. A file system `rm` removes the inode but leaves data on disk. For Restricted data, overwrite or crypto-erase is essential.

> **⚠️ Watch out:** Backup immutability working against you. Immutable backups protect against ransomware but also prevent intentional deletion. If your backup retention is 7 years, customer data in backups persists for 7 years after the customer leaves. Design backup retention to support your deletion commitments.

> **⚠️ Watch out:** "Delete" in distributed systems. Deleting a row from a primary database doesn't automatically delete it from: read replicas, materialized views, search indexes (Elasticsearch), CDN caches, analytics data warehouses, and log archives. Map your data flow before committing to deletion SLAs.

---

## Procedure 4: Monitoring Setup for Data Protection

### Standard Approach

1. **Monitoring Scope Definition**
   - Identify all production systems that store, process, or transmit Restricted and Confidential data.
   - For each system, define the monitoring baseline: what events signal potential data misuse or exfiltration.

2. **Enable Core Monitoring Controls**
   - **Activity Monitoring:** Enable database audit logging, application-level access logging, and API gateway request logging. Forward all logs to SIEM (`____`).
   - **File Integrity Monitoring (FIM):** Deploy FIM on servers storing sensitive data. Monitor for unauthorized changes to data files, configuration files, and encryption certificates. Alert on changes outside approved change windows.
   - **Vulnerability Scanning:** Run authenticated vulnerability scans against all production systems on a `____` (weekly) schedule. Flag and remediate vulnerabilities per the Vulnerability Management Policy.
   - **Malware Detection:** Ensure EDR/anti-malware is deployed on all systems as described in the Acceptable Use Procedures.

3. **Data-Specific Detection Rules**
   Configure the following detection rules in the SIEM:
   - **Bulk Data Access:** SELECT or DOWNLOAD operations returning >`____` rows or >`____` MB of data from Restricted data stores.
   - **Off-Hours Data Access:** Access to Restricted data stores outside `____` to `____` (business hours).
   - **First-Time Access:** A user accessing a Restricted data store they have never accessed before.
   - **Privileged User Data Access:** Any database administrator or privileged account directly querying customer data tables (as opposed to system administration operations).
   - **Data Exfiltration Patterns:** Outbound network traffic >`____` MB from a production database server to an external IP, or DNS queries for domains associated with data exfiltration tools.
   - **Encryption Disablement:** Any attempt to disable or modify encryption settings on data stores.

4. **Alert Triage and Response**
   - **Severity Critical:** Immediate page to on-call Security Engineer (15-minute acknowledgement SLA). Examples: confirmed data exfiltration, encryption disabled.
   - **Severity High:** Ticket created with 1-hour acknowledgement SLA. Examples: off-hours bulk data access by privileged user.
   - **Severity Medium:** Ticket created with 24-hour review SLA. Examples: first-time access to Restricted data.

5. **Monitoring Health Checks**
   - Daily: confirm all data store log sources are actively forwarding to SIEM.
   - Weekly: verify that detection rules are firing (not silently broken).
   - Monthly: tune detection thresholds based on false positive rates; aim for <`____`% false positive rate.

### Alternative Approaches

> **💡 Why you might choose differently:** Data protection monitoring varies significantly by infrastructure and risk profile.

- **UEBA (User and Entity Behavior Analytics):** Deploy UEBA on top of SIEM (e.g., Splunk UBA, Microsoft Sentinel UEBA, Exabeam) to baseline normal user behavior and detect deviations automatically — no manual rule writing needed. Better for large environments where manual rule maintenance becomes unsustainable.
- **CSPM + DSPM (Data Security Posture Management):** Use cloud-native DSPM tools (e.g., Laminar, Dig Security) that automatically discover data stores, classify data, and monitor for misconfigurations and anomalous access. Particularly valuable for multi-cloud environments.
- **Database Activity Monitoring (DAM) Appliance:** Deploy a dedicated DAM solution (e.g., Imperva, IBM Guardium) for real-time SQL traffic monitoring with blocking capability. Useful for high-compliance environments where you want to BLOCK suspicious queries, not just alert.

### Common Pitfalls

> **⚠️ Watch out:** Log volume cost. Full query logging on a high-throughput production database can generate terabytes of logs daily. Balance completeness with cost: log all access to Restricted tables, but consider sampling or summarizing access to Internal/Public data.

> **⚠️ Watch out:** Monitoring blind spots. If a developer SSHes into a production server and runs `mysqldump` locally, the database audit log captures it — but if they `scp` the dump file off the server, do your network logs capture it? Map your detection coverage across all exfiltration paths.

> **⚠️ Watch out:** Alert fatigue from overly broad rules. "Any access to Restricted data" generates hundreds of alerts per hour in a functioning application. Write rules that capture ANOMALOUS access, not all access.

---

## Related Documents

- Data Protection Policy (Template.md)
- Information Security Policy (ISP-Template.md)
- Data Classification Policy
- Data Retention Policy
- Encryption Policy
- Logging and Monitoring Policy
- Acceptable Use Procedures (AUP-Procedures.md) — for malware detection and EDR deployment
- Incident Response Process (IR-Process-Template.md)
