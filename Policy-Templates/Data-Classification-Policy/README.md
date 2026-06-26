# Data Classification Policy Template

## What This Is

The Data Classification Policy defines how the organization categorizes its data by sensitivity and what security controls apply at each level. Everything else — encryption, access control, backup, retention — uses these classifications to decide what level of protection to apply.

## What It Covers

- Four-level classification scheme (Restricted, Confidential, Internal, Public)
- Classification assignment responsibilities
- Handling controls matrix (encryption, access, sharing, disposal)
- De-identification requirements
- Reclassification and review process

## Gotchas People Get Wrong

**1. Default classification matters.** If "Internal" is the default and your engineers default-save everything, then nothing gets the encryption or access controls it should. Make sure your default classification and your actual data handling practices align.

**2. "Internal" becomes a dumping ground.** When people don't know how to classify something, they pick Internal. This means your most sensitive data might be sitting in Internal-labeled S3 buckets. Regular classification reviews are essential.

**3. End-user classification doesn't work without automation.** Expecting employees to manually classify every file they create is unrealistic. Automate classification where possible: tag cloud resources, use DLP tools to detect PII, classify by data store rather than individual files.

**4. The handling controls matrix looks great on paper but fails in practice.** "Encryption Required for Restricted data" — how do you enforce this? If you can't enforce it, it's not a control, it's a suggestion. Each row in the matrix needs a technical enforcement mechanism.

**5. Four levels is the sweet spot.** Three (Public, Internal, Restricted) is workable for small orgs. Five or more gets confusing and people stop classifying correctly. Four (Public, Internal, Confidential, Restricted) is optimal for most mid-size companies.

## Implementation Advice

- **Start by classifying data stores, not individual files.** Classify your S3 buckets, databases, SharePoint sites, and Google Drive shared drives. Individual files inherit the classification of their container. This is 80% of the value for 20% of the effort.
- **Your data classification scheme drives your backup and retention policies.** Restricted data might need daily backups retained 7 years. Public data might need no backup at all. These policies talk to each other.
- **PII auto-detection is worth the investment.** Tools that scan for credit card numbers, SSNs, and other PII patterns catch misclassified data. AWS Macie, Google DLP, and similar tools pay for themselves the first time they find PII in a public bucket.
- **Train on classification at onboarding and annually.** Most people don't know the difference between Confidential and Restricted. Use real examples from your business.
