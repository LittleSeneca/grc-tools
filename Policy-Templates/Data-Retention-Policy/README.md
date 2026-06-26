# Data Retention Policy Template

## What This Is

The Data Retention Policy defines how long the organization keeps different types of data and what happens when the retention period expires. It's one of the hardest policies to implement correctly because it requires coordination between legal, engineering, and operations teams.

## What It Covers

- Retention principles (keep only what's needed)
- Retention schedule by data category
- Data deletion requirements and methods
- Legal hold procedures
- Backup and archive retention considerations

## Gotchas People Get Wrong

**1. Backups make deletion nearly impossible.** When you delete a customer record from your production database, that record still exists in nightly backups for the next 30/60/90 days. True "deletion" means waiting for the backup retention period to expire. Be honest about this in your policy — don't claim you can delete data from backups that you can't touch.

**2. "Delete within 90 days of account closure" means 90 days plus your backup window.** If customers can request deletion and you have 30-day backups, your policy should say "data will be fully purged within 120 days (90-day processing window + 30-day backup cycle)." Transparency prevents disputes.

**3. Legal holds override everything.** When litigation starts, your retention policy goes out the window — you must preserve everything relevant. Make sure you have a process to rapidly identify and freeze data when a legal hold arrives. Screenshots and manual processes don't scale.

**4. Employee data retention varies by jurisdiction.** US states, EU member countries, and other jurisdictions have different requirements for how long you must retain (and must not retain) employee data. A single "7 years" policy may not work globally.

**5. "Retain for 7 years" without a deletion mechanism is just data hoarding.** If your policy says "retain financial records for 7 years," you need a system that automatically deletes them after 7 years + 1 day. Manual deletion at scale is a fantasy.

## Implementation Advice

- **Automate deletion where you can.** S3 lifecycle policies, database TTL features, and log rotation tools can handle most retention policies automatically. Manual processes break down at scale.
- **The retention schedule is a living document.** Laws change, contracts change, business needs change. Review the retention schedule annually with legal counsel.
- **Test your deletion process before you need it.** Pick a non-critical data category, run through the full deletion workflow, and verify the data is actually gone (including backups). You'll find gaps you didn't expect.
- **Data inventory makes retention possible.** If you don't know where your data lives, you can't apply retention policies to it. The Data Classification Policy's inventory is the foundation for this policy.
