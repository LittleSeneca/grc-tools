# Data Protection Policy Template

## What This Is

The Data Protection Policy defines the technical and procedural controls that protect data throughout its lifecycle. It's where the abstract classification levels from the Data Classification Policy meet actual implementation: encryption, access control, monitoring, and disposal.

## What It Covers

- Customer data protection requirements
- Production access controls (least privilege, just-in-time)
- Data separation and multi-tenancy
- Monitoring and alerting for production systems
- NDA/confidentiality agreement requirements
- Data at rest (encryption, storage, deletion)
- Data in transit (encryption, transfer risk assessment)
- Third-party information exchange
- Secure messaging requirements

## Gotchas People Get Wrong

**1. "No direct production access" is aspirational unless you enforce it.** Everyone writes this, but if developers have IAM keys that can reach production databases, the policy is fiction. Implement just-in-time access with approval workflows, or at minimum, alert on direct production access.

**2. Data deletion is harder than it sounds.** "Properly deleted" for cloud data requires understanding how your provider handles deletion. AWS S3 deletion is eventually consistent. Database records may exist in backups for months. Cloud provider "delete" APIs may not be immediate. Your deletion policy needs to account for these realities.

**3. The NDA section is often boilerplate.** Don't just copy-paste NDA requirements. Make sure your actual NDA/employment agreements match what the policy says they should contain. An auditor may ask to see a signed NDA and compare it to your policy.

**4. "End-to-end encryption for sensitive email" is operationally hard.** Most email is not end-to-end encrypted. If you require it for sensitive data, you need to provide the tool (ProtonMail, encrypted attachments, secure portals). Otherwise, people just send sensitive data unencrypted and don't tell you.

**5. Data separation at the application layer is the right approach, but it must be proven.** If your policy says "customer data is logically separated," be ready to show how. API-level enforcement with tenant-scoped queries is the standard pattern. Database-level separation (separate DB per customer) is stronger but operationally heavier.

## Implementation Advice

- **Just-in-time access is the single biggest improvement you can make.** If you implement nothing else from this policy, implement JIT access for production. It changes production access from a persistent risk to an audited, temporary event.
- **Monitor what you can't prevent.** If you can't block direct production access, at minimum alert on it and review it weekly. An unreviewed alert feed is noise; a weekly review of production access patterns catches real issues.
- **Deletion records are evidence.** When you delete customer data, log: who authorized it, who performed it, what was deleted, when, and using what method. This is your proof if a customer later claims you still have their data.
- **The messaging section needs a practical, approved tool.** Don't just say "don't send sensitive data over email." Give people an approved alternative: "Use [secure file sharing tool] for files containing PII. It takes 30 seconds."
