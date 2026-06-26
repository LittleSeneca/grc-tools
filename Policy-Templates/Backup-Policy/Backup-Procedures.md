# Backup Policy — Implementation Procedures

> **Companion to:** Backup Policy (Template.md)
> **Purpose:** These procedures describe how to implement the backup requirements defined in the Backup Policy.

## Procedure: Configuring the 3-2-1 Backup Framework

### Standard Approach

1. **Inventory critical data assets.** List all production databases, file stores, object storage buckets, configuration repositories, and source code repositories that fall within backup scope.
2. **Designate three copy locations:**
   - **Copy 1 (Production):** The live data as used by applications and users.
   - **Copy 2 (Primary Backup):** A backup in the same cloud region or on-premises facility, stored on different infrastructure than production (e.g., separate storage account, separate SAN).
   - **Copy 3 (Off-Site Backup):** A backup in a geographically separate region (≥ ____ miles from primary, or a different cloud provider region). This copy must be in a separate failure domain — a regional outage affecting the primary site must not affect this copy.
3. **Select two different media types.** Common pairings:
   - Cloud object storage (e.g., S3, GCS, Azure Blob) + cloud cold storage tier (e.g., Glacier, Archive)
   - Cloud object storage + on-premises NAS (for hybrid environments)
   - Cloud block storage snapshots + cloud object storage exports
4. **Configure replication or backup jobs** to create all copies automatically. Validate that each copy is written to the intended storage target and that no single credential set can delete all three copies.

### Alternative Approaches

> **💡 Why you might choose differently:** The strict 3-2-1 rule can be expensive for large datasets or low-criticality data. Adjust the model based on data classification.

- **Alternative A — 3-2-1 with tiering:** Apply 3-2-1 only to Restricted and Confidential data. Internal data gets 2 copies on 2 media types (2-2-0). Public data gets 1 copy.
- **Alternative B — Immutable backups:** Use WORM (Write Once Read Many) storage for Copy 3 to protect against ransomware that attempts to delete or encrypt backups. This satisfies the spirit of 3-2-1 with stronger protection on the off-site copy.
- **Alternative C — Cloud-native snapshots:** For fully cloud-native organizations, rely on cross-region snapshot replication + cloud provider backup service instead of managing separate media types.

### Common Pitfalls

> **⚠️ Watch out:** All three copies stored in the same AWS account under the same root credentials do NOT satisfy 3-2-1. A compromised admin account can delete everything. Use separate accounts or a separate cloud provider for Copy 3.
>
> **⚠️ Watch out:** "Different media types" doesn't mean two S3 buckets in the same region — they share a failure domain. If you're cloud-only, use different storage classes (e.g., S3 Standard + S3 Glacier Deep Archive) in different regions, with different access credentials.

---

## Procedure: Configuring Database Backup Frequency

### Standard Approach

1. **Identify the database engine** (PostgreSQL, MySQL, SQL Server, MongoDB, etc.) and its native backup tools (`pg_dump`, `mysqldump`, native snapshot APIs).
2. **Configure daily full backups:**
   - Schedule during the lowest-activity window (typically 01:00–04:00 local time).
   - Use the database's native consistent backup mechanism (e.g., `pg_dump -Fc` for PostgreSQL, snapshots for cloud-managed databases).
   - Validate backup completion with a post-backup hook that checks file size > 0 and exit code 0.
3. **Configure transaction log backups:**
   - Set interval at ____ minutes (recommended: 15 minutes for critical databases, 60 minutes for non-critical).
   - Use WAL archiving (PostgreSQL), binary log streaming (MySQL), or cloud-native point-in-time recovery.
   - Monitor log backup lag — if lag exceeds 2× the configured interval, trigger an alert.
4. **Verify backup integrity** using a scheduled job that attempts to restore the most recent backup to a sandbox environment and run a consistency check (`pg_restore --schema-only` or equivalent).

### Alternative Approaches

> **💡 Why you might choose differently:** Native dump tools don't scale well for multi-terabyte databases.

- **Alternative A — Cloud-managed automated backups:** Use the cloud provider's managed backup service (RDS automated backups, Cloud SQL backups). This eliminates tooling maintenance but ties you to the provider's restore process.
- **Alternative B — Streaming replication:** For databases requiring near-zero RPO, maintain a hot standby with streaming replication in a different region. This supplements (not replaces) logical backups — replication replicates mistakes and ransomware too.
- **Alternative C — Storage-level snapshots:** Use filesystem or block-storage snapshots for very large databases where logical dumps take too long. Ensure the snapshot is taken with the database in a consistent state (quiesce or freeze first).

### Common Pitfalls

> **⚠️ Watch out:** `pg_dump` on a 2 TB database can take 8+ hours — you might not finish before the next backup window. Test backup duration at production scale before committing to a frequency.
>
> **⚠️ Watch out:** Transaction log backups that succeed but are never tested for replay can create a false sense of security. At least quarterly, test a point-in-time restore using the log chain.

---

## Procedure: Source Code Backup and Commit Preservation

### Standard Approach

1. **Identify all source code repositories** (GitHub, GitLab, Bitbucket, self-hosted Git).
2. **Configure a daily mirror job** for every repository:
   ```bash
   git clone --mirror <repo-url> /backup/path/repo.git
   cd /backup/path/repo.git
   git remote update
   ```
3. **Store mirrors in organization-controlled object storage** (separate from the hosting provider). Example: a daily CronJob in Kubernetes or scheduled AWS Lambda that:
   - Clones/mirrors all repos
   - Tars and encrypts the bundle
   - Uploads to a separate cloud object storage bucket with versioning enabled
4. **Preserve every commit:** The mirror includes the full commit history. Configure the object storage bucket with versioning so that even if a subsequent backup job overwrites data, previous versions are recoverable.
5. **Verify integrity quarterly:** Clone from the backup mirror and confirm `git fsck` reports no errors.

### Alternative Approaches

> **💡 Why you might choose differently:** Mirroring every repo daily is overkill if your hosting provider already has strong durability guarantees.

- **Alternative A — Provider-native export:** Use GitHub/GitLab's built-in export/migration API to download a weekly archive. Less frequent but simpler.
- **Alternative B — Self-hosted Gitea mirror:** Maintain a self-hosted Gitea instance that mirrors all repos in near-real-time. Functions as both a backup and a failover option.
- **Alternative C — CI/CD artifact backup:** If your CI/CD pipeline already produces build artifacts, back those up instead of raw source. Faster restore for deployment, but you lose commit history.

### Common Pitfalls

> **⚠️ Watch out:** A `git clone` (non-mirror) only fetches the default branch. Use `--mirror` to capture all branches, tags, and refs.
>
> **⚠️ Watch out:** Large repositories with many binary assets (LFS objects) require special handling. `git clone --mirror` doesn't automatically fetch LFS objects — use `git lfs fetch --all` after mirroring.

---

## Procedure: Restore Testing

### Standard Approach

1. **Schedule quarterly restore tests.** Create a recurring calendar event and assign ownership to ____.
2. **For each test cycle, select a sample** of backup types to restore:
   - At least one production database from each database engine
   - One file/object storage backup
   - One configuration repository
   - One source code repository
3. **Restore to an isolated environment:**
   - Use a separate cloud account, VPC, or on-premises sandbox that has no network connectivity to production.
   - Provision minimal infrastructure from infrastructure-as-code templates (do not connect to production networks).
4. **Validate the restored data:**
   - **Database:** Run schema validation, row counts, and a sample query against known reference data.
   - **File storage:** Verify file counts, checksums against a known manifest, and spot-check file contents.
   - **Configuration:** Confirm that the configuration can be parsed/validated by the target system.
   - **Source code:** Run `git fsck` and confirm a build succeeds from the restored code.
5. **Document results** in the Backup Integrity Testing Process repository, including:
   - Systems restored, backup dates used, time to restore
   - Validation results (pass/fail)
   - Any failures or anomalies
6. **Remediate failures within ____ business days** and re-test.

### Alternative Approaches

> **💡 Why you might choose differently:** Full quarterly restore testing of everything is operationally expensive.

- **Alternative A — Rotating schedule:** Test different systems each quarter so all critical systems are tested at least annually. Reduces per-quarter effort.
- **Alternative B — Automated restore validation:** For each backup job, automatically restore to a sandbox, run validation scripts, and tear down. Only flag for manual review if validation fails. This makes "quarterly testing" continuous.
- **Alternative C — Chaos engineering approach:** Periodically (e.g., annually) run a full-scale DR exercise where you intentionally fail over to backup-restored systems. Tests both restore and failover procedures simultaneously.

### Common Pitfalls

> **⚠️ Watch out:** Restoring a backup and seeing "no errors" in the restore tool is NOT sufficient validation. The restored database might have all rows but corrupted data within them. Always run application-level validation.
>
> **⚠️ Watch out:** If you always restore the same database during testing, you're only testing that database's backup pipeline. Rotate which systems you restore.
>
> **⚠️ Watch out:** Restoring to the same server that hosts production can accidentally overwrite live data. Always use an isolated environment.

---

## Procedure: Backup Monitoring and Alerting Setup

### Standard Approach

1. **Instrument every backup job** with success/failure logging:
   - Wrap backup scripts with logging that writes structured output (JSON or key=value) to a central log aggregator.
   - Include: job name, start time, end time, exit code, data size, destination.
2. **Configure alerting rules:**
   - **Backup failure:** Alert immediately (within ____ minutes) to ____ team via messaging platform + ticketing system auto-create.
   - **Backup job not started:** Alert if a scheduled job hasn't started within ____ minutes of its scheduled time (detects scheduler failures).
   - **Backup size anomaly:** Alert if backup size drops by >____% or increases by >____% compared to the 7-day rolling average (detects silent data loss or unexpected data growth).
   - **Backup storage approaching capacity:** Alert when backup storage exceeds ____% of allocated capacity.
3. **Create a backup health dashboard** showing:
   - Last success time for each backup job (color-coded: green <24h, yellow 24-48h, red >48h)
   - Total backup storage used vs. allocated
   - Restore test status (last test date, pass/fail)
4. **Monthly capacity review:** On the first business day of each month, ____ reviews backup storage consumption, retention compliance, and any capacity trends.

### Alternative Approaches

> **💡 Why you might choose differently:** Full custom monitoring may be overkill if your backup tool already provides it.

- **Alternative A — Backup vendor monitoring:** Use the monitoring and alerting built into your backup platform (Veeam, Rubrik, Commvault, cloud-native backup services). Supplement with a periodic manual review.
- **Alternative B — Infrastructure-as-code monitoring:** Define backup monitoring rules in Terraform/CloudFormation alongside the backup infrastructure. Changes to backup configuration automatically update monitoring.
- **Alternative C — Simple cron email:** For small organizations, a cron job that emails results to a shared mailbox may suffice. Less robust but very low overhead.

### Common Pitfalls

> **⚠️ Watch out:** Monitoring that a backup job "ran" is not the same as monitoring that it "succeeded." A job can exit code 0 but produce a 0-byte backup file. Always validate output.
>
> **⚠️ Watch out:** Alert fatigue from non-critical backup warnings (e.g., "backup took 2 minutes longer than usual") trains people to ignore all backup alerts. Reserve immediate alerts for failures only; use dashboards for trends.

---

## Procedure: Backup Encryption Configuration

### Standard Approach

1. **Encrypt data before it leaves the source system.** Use client-side encryption with a key managed by the organization's key management service (KMS), not the backup destination's default encryption.
2. **For cloud object storage backups:**
   - Enable server-side encryption with customer-managed keys (SSE-C or SSE-KMS with CMK).
   - Create a dedicated KMS key for backups that is separate from the production data encryption key.
   - Configure the key policy so that only the backup service account and designated recovery personnel can use it for decryption.
3. **For on-premises or hybrid backups:**
   - Encrypt backup files with AES-256-GCM using a key derived from the organization's KMS or HSM before writing to backup media.
   - Never store the encryption key alongside the backup data.
4. **Validate encryption quarterly** by attempting to restore a backup in an isolated environment without the decryption key — it should fail. Then restore with the key — it should succeed.

### Alternative Approaches

> **💡 Why you might choose differently:** Client-side encryption adds latency and CPU overhead for large backup volumes.

- **Alternative A — Storage-level encryption only:** Rely on server-side encryption provided by the storage platform. Simpler but means the storage provider technically has access to the encryption keys. Acceptable for Internal data; not recommended for Restricted data.
- **Alternative B — Backup appliance encryption:** Use a dedicated backup appliance that handles encryption transparently. The appliance manages keys internally.

### Common Pitfalls

> **⚠️ Watch out:** If the KMS key used to encrypt backups is in the same AWS account as the backups and the account is compromised, the attacker can decrypt everything. Store backup encryption keys in a separate account with cross-account access.
>
> **⚠️ Watch out:** Encrypting backups with a key that has an automatic rotation policy without preserving old key versions renders old backups unrecoverable. Always keep previous key versions active for the duration of your retention period.

## Related Documents

- Backup Policy (Template.md)
- Backup Integrity Testing Process
- Disaster Recovery Plan
- Disaster Recovery Process
- Encryption Policy
- Data Classification Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
