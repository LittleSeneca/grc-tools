# Remediation Plan Template

## What This Is

The Remediation Plan defines the operational process for fixing security findings — from discovery to verification to closure. It's the bridge between "we found a vulnerability" and "we fixed it." The SLAs in this document are what auditors check to determine if your vulnerability management program is actually effective.

## What It Covers

- Finding identification and documentation
- Prioritization and severity-based SLAs
- Assignment to responsible teams
- Remediation action types (remediate, mitigate, transfer, accept)
- Verification that fixes are effective
- Exception handling for SLA misses
- Reporting to oversight body

## Gotchas People Get Wrong

**1. SLA timelines that look good on paper but are impossible to meet.** "Critical vulnerabilities: 7 days" sounds responsible until you realize it takes 3 days just to get through your change management process. Align your SLAs with your actual ability to deploy fixes. A 14-day SLA you consistently meet is better than a 7-day SLA you regularly miss.

**2. "Accept" becomes the default for hard-to-fix findings.** When a vulnerability is difficult or expensive to fix, there's pressure to just accept the risk. Track your acceptance rate — if it's trending up, you're using acceptance to avoid hard work, not because the risk calculus actually supports it. Require formal risk acceptance with documented justification.

**3. Verification is the most commonly skipped step.** After applying a patch, someone should verify the vulnerability is actually gone. Often, teams apply the fix, close the ticket, and move on — only for the next scan to re-open the same finding. Make re-scanning part of the closure criteria.

**4. Separation of duties in verification matters.** If the person who applied the fix also verifies it, you have no independent check. For high and critical findings, the verifier should be a different person (or an automated scan, which is inherently independent).

**5. Exception tracking falls apart without enforced review.** If you create an exception with a 30-day extension, someone needs to follow up on day 31. Without automated reminders and escalation for overdue exceptions, exceptions become permanent.

## Implementation Advice

- **Automate the link between findings and tickets.** When your vulnerability scanner finds something, it should automatically create a ticket in your tracking system with the severity, affected system, and remediation guidance. Manual transcription wastes time and introduces errors.
- **Use dashboards, not reports.** A real-time dashboard showing open findings by severity, SLA status, and aging is more actionable than a monthly PDF report. The oversight body should see the same dashboard.
- **Track mean time to remediate (MTTR) as a metric.** Average time from finding to closure, broken down by severity. This is your program's North Star metric. If MTTR is increasing, you're losing ground.
- **Acceptance requires annual re-evaluation.** A risk accepted today may not be acceptable next year if the threat landscape changes or your risk appetite shifts. Every accepted risk must have an annual review date.
