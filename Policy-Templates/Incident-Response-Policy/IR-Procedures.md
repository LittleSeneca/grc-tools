# Incident Response Policy — Implementation Procedures

> **Companion to:** Security Incident Response Policy (IR-Policy-Template.md)
> **Purpose:** These procedures describe how to implement the incident response requirements defined in the policy. They focus on IRT mobilization, incident severity classification, evidence handling, and post-incident activities not covered in detail by the Incident Response Process document. For the full step-by-step operational IR process, refer to the Security Incident Response Process.

## Procedure: IRT Mobilization and Readiness

### Standard Approach

#### Maintaining IRT Readiness

1. **Maintain the IRT contact roster:**
   - Store in at least two independent locations (e.g., password manager + printed copy + HR system).
   - Include: name, role, mobile number, personal email (for when corporate email is down), and home address (for physical muster if needed).
   - Verify accuracy quarterly — call each number to confirm it reaches the right person.
2. **Configure communication channels:**
   - **Primary:** Dedicated IRT channel in the organization's messaging platform with all IRT members. This channel must exist in a workspace that is NOT dependent on the same SSO/infrastructure as production (use a separate account or a consumer-grade backup like Signal group).
   - **Secondary:** SMS distribution list. Test monthly by sending a test message.
   - **Tertiary:** Phone tree. Each person calls the next; the last calls the first to confirm loop closure.
3. **Verify tool readiness quarterly:**
   - Forensic toolkit: can you image a disk, capture memory, and analyze logs?
   - Evidence storage: is the secure evidence repository accessible and writable?
   - Break-glass accounts: do the emergency admin accounts work? Rotate credentials after each use.
4. **Conduct tabletop exercises** at least semi-annually (the IR Process requires quarterly plan review; exercises should be part of that cycle).

#### Mobilizing the IRT (During an Incident)

1. **First responder detects or receives report of a potential incident.**
2. **Simultaneously notify via ALL channels:**
   - Post in the IRT messaging channel: "POTENTIAL INCIDENT — [brief description]. Assembling team now."
   - Send SMS to the IRT Lead with the same message.
   - Call the IRT Lead directly.
3. **If the IRT Lead does not acknowledge within ____ minutes:**
   - Move to the next person in the chain of command (Security Officer → Engineering Lead → Legal/Operations Officer).
   - Continue down the chain until someone acknowledges and assumes command.
4. **The IRT Lead (or successor) declares the incident** and assigns initial severity. From this point, all actions are documented in the incident ticket.
5. **Assemble the full IRT:**
   - Continue contacting team members through all channels until everyone is in the dedicated communication channel.
   - Anyone who cannot be reached within ____ minutes is noted; if their role is critical, escalate to find them (call their emergency contact, if listed).

### Alternative Approaches

> **💡 Why you might choose differently:** The "call everyone immediately" approach can be unnecessarily disruptive for low-severity incidents.

- **Alternative A — Tiered mobilization:** For Medium/Low severity, notify only the IRT Lead and Security Officer initially. They decide whether to mobilize the full team. For Critical/High, full mobilization is automatic.
- **Alternative B — On-call rotation:** Designate an on-call IRT Lead and Security Officer each week. Only the on-call personnel are paged; they assemble the team if needed. Reduces alert fatigue.

### Common Pitfalls

> **⚠️ Watch out:** The IRT messaging channel is inside the corporate Slack/Teams workspace that's SSO-gated. If the identity provider is compromised or down, nobody can access the channel. Always maintain a backup communication path (Signal, WhatsApp, SMS group) that's independent of corporate infrastructure.
>
> **⚠️ Watch out:** "I sent a message to the channel" is not mobilization. Until someone acknowledges and assumes command, you have not mobilized. Keep escalating until you get a human response.
>
> **⚠️ Watch out:** The IRT contact roster has the CISO's old phone number from three years ago. When you test quarterly, actually call the numbers — don't just email asking people to confirm their info.

---

## Procedure: Incident Severity Classification

### Standard Approach

The IRT Lead (or Security Officer) assigns severity based on the following decision tree:

1. **Is there an active attacker in the environment RIGHT NOW?**
   - YES → **Critical.** Full IRT mobilization within 1 hour.
2. **Has a confirmed data breach occurred involving Restricted or Confidential data?**
   - YES → **Critical** if data is exfiltrated or attacker had access. Full mobilization.
3. **Are core business operations disrupted?**
   - YES, affecting customers → **Critical** or **High** (based on scope: widespread = Critical, limited = High).
   - YES, internal only → **High** or **Medium**.
4. **Is there confirmed unauthorized access but no data exfiltration or operational disruption?**
   - **High** if sensitive systems were accessed.
   - **Medium** if non-sensitive systems only.
5. **Is there a clear policy violation with no operational impact?**
   - **Medium** (e.g., unauthorized software installation, minor configuration drift with security implications).
6. **Is this a single event with no indicators of compromise and no business impact?**
   - **Low.** Log, track, and review for patterns.

#### Severity Escalation

- **If in doubt, escalate.** A Medium incident that's later found to be Critical is far worse than a Critical incident that was over-classified.
- **Re-classify as new information emerges.** Severity is not fixed at declaration. If the investigation reveals the breach is larger than initially thought, escalate immediately.
- **De-escalation requires IRT Lead approval.** Document the rationale.

### Common Pitfalls

> **⚠️ Watch out:** The tendency is to under-classify ("let's not overreact"). This delays mobilization and containment. The policy explicitly says: "if any reviewing member of the IRT is unsure whether a notification constitutes an incident, assume it is an incident."
>
> **⚠️ Watch out:** Severity classification based on "what we know right now" is fine, but if "what we know right now" is almost nothing, default to the worst reasonable case until you have evidence to downgrade.

---

## Procedure: Evidence Handling and Chain of Custody

### Standard Approach

> **Note:** The Incident Response Process covers what evidence to collect. This procedure covers HOW to collect and preserve it properly.

1. **Designate an evidence custodian** as soon as the incident is declared. This person tracks all evidence from collection to storage.
2. **Collect evidence in order of volatility** (most volatile first):
   - Memory (RAM captures) — lost on reboot
   - Network connections and running processes — change constantly
   - System logs in memory buffers — may not be written to disk
   - Disk images — persist across reboots but can be overwritten
   - Backup tapes / snapshots — generally stable
3. **For each piece of evidence, record:**
   - Unique identifier (e.g., INC-2024-001-EV-001)
   - Description (what it is, where it came from)
   - Date and time of collection
   - Person who collected it
   - Method of collection (tool and command used)
   - Hash value (SHA-256) immediately after collection
4. **Store evidence securely:**
   - Use a dedicated, access-controlled evidence repository (cloud storage bucket with strict IAM, or physical secure storage).
   - Encrypt evidence at rest.
   - Limit access to the IRT Lead, Security Officer, and evidence custodian.
5. **Log every transfer of custody:**
   - Who transferred it, to whom, when, and why.
   - Verify hash before and after transfer to confirm integrity.
6. **Retain evidence per policy** (minimum ____ years, longer if litigation is anticipated).

### Alternative Approaches

> **💡 Why you might choose differently:** Full forensic evidence collection is time-consuming and may delay containment.

- **Alternative A — Triage-first approach:** Collect only enough evidence to understand the incident and contain it. Full forensic collection happens after containment, if needed. Balances speed with thoroughness.
- **Alternative B — Cloud-native evidence collection:** For cloud environments, use the cloud provider's native forensic capabilities (e.g., AWS CloudTrail, disk snapshots, memory dumps via API) rather than traditional forensic tools. Faster and doesn't require physical access.

### Common Pitfalls

> **⚠️ Watch out:** Rebooting a compromised system to "fix it" destroys volatile evidence (memory, running processes, network connections). Contain first without rebooting if possible; capture memory before any reboot.
>
> **⚠️ Watch out:** Evidence stored in a shared Google Drive folder with "anyone with the link can view" is not evidence — it's a data breach on top of your incident. Chain of custody means controlled access, always.
>
> **⚠️ Watch out:** Screenshots are easy but are the weakest form of evidence (easily altered, no hash verification). Use them for quick documentation but supplement with disk images, memory captures, and log exports for forensic-grade evidence.

---

## Procedure: Post-Incident Debug and Lessons Learned

### Standard Approach

#### Conducting the Debrief

1. **Schedule within ____ business days** of incident resolution (recommended: within 5 days, while memories are fresh).
2. **Participants:** All IRT members who participated in the response, plus any external parties (DFIR firm, legal counsel, cyber insurance team).
3. **Agenda:**
   - Timeline walkthrough (use the incident log and ticket).
   - What worked well? (Celebrate successes — this is important for team morale.)
   - What didn't work? (Be specific: which procedure, tool, or communication step failed?)
   - Were response timelines met? (Compare actual vs. SLA: time to detect, time to contain, time to eradicate, time to recover.)
   - Communication effectiveness: was internal communication clear and timely? External?
   - Tools and resources: were they sufficient and functional?
   - If external support was engaged: was coordination effective?
4. **Ground rules:**
   - This is a blameless process. Focus on systems and processes, not individuals.
   - "What could WE do differently next time?" — not "Who messed up?"

#### Documenting Lessons Learned

1. **Create a Lessons Learned document** within ____ business days of the debrief.
2. **Structure:**
   - Incident summary (1 paragraph).
   - Timeline of key events.
   - What worked / what didn't (from the debrief).
   - Root cause (from RCA).
   - Corrective actions (specific, measurable, assigned, with due dates).
   - Recommendations for policy or process changes.
3. **Distribute to:**
   - All IRT members.
   - Executive leadership.
   - Security Oversight Committee.
4. **Track corrective actions** in the ticketing system. Review progress at the next Security Oversight Committee meeting.

#### User Notification and Training

1. **If the incident involved user-correctable behavior** (e.g., someone clicked a phishing link):
   - Send an organization-wide notification explaining what happened, what users should look for, and what they should do differently.
   - Do NOT name or shame the individual — this discourages future reporting.
2. **If the incident reveals a training gap:**
   - Update security awareness training materials.
   - Schedule a supplemental training session if urgent.

### Common Pitfalls

> **⚠️ Watch out:** The lessons learned meeting gets postponed "until things calm down" and never happens. Schedule it before the incident is even fully resolved — put it on the calendar as a hard commitment.
>
> **⚠️ Watch out:** Corrective actions like "improve monitoring" are too vague to be actionable. Make them specific: "Add alert for any IAM user creation outside of Terraform, with notification to #irt-alerts within 5 minutes."
>
> **⚠️ Watch out:** Lessons learned documents that sit in a Confluence page nobody reads are worthless. Present the top 3 findings at the next all-hands or team meeting to ensure organizational learning.

---

## Procedure: Regulatory Notification (Breach Scenarios)

### Standard Approach

> **Note:** This procedure is invoked when the incident involves a confirmed breach of personal data or regulated data. It supplements the communication procedures in the IR Process.

1. **Determine notification obligations immediately upon confirming a breach:**
   - What type of data was involved? (PII, PHI, PCI, etc.)
   - Which regulations apply? (GDPR, CCPA/CPRA, HIPAA, PCI DSS, state breach notification laws)
   - What are the notification deadlines?
2. **Critical deadlines (reference):**
   - **GDPR:** Notify supervisory authority within 72 hours of becoming aware. Notify affected data subjects without undue delay if high risk.
   - **CCPA/CPRA:** Notify affected consumers "without unreasonable delay."
   - **HIPAA:** Notify affected individuals within 60 days of discovery. Notify HHS within 60 days (for breaches affecting 500+ individuals, notify HHS contemporaneously).
   - **PCI DSS:** Per acquiring bank and card brand requirements (typically within 24 hours).
   - **US State laws:** Vary by state (typically 30–60 days, some as short as 72 hours).
3. **Engage legal counsel** before any regulatory notification. The Legal/Operations Officer coordinates this.
4. **Prepare the notification:**
   - What happened (nature of the breach).
   - What data was involved.
   - What the organization is doing in response.
   - What affected individuals should do.
   - Contact information for questions.
5. **Deliver notification** through the required channels (email, postal mail, substitute notice if contact information is insufficient).
6. **Document everything:** When notification was made, to whom, through what channel, and the content of the notification. This documentation is critical for regulatory defense.

### Common Pitfalls

> **⚠️ Watch out:** The GDPR 72-hour clock starts ticking when the organization becomes "aware" of the breach — not when the investigation is complete. If you know a breach occurred but don't know the full scope, notify the supervisory authority with what you know and update later.
>
> **⚠️ Watch out:** Notifying affected individuals before law enforcement has been engaged can compromise an investigation. Coordinate timing with law enforcement if applicable.

## Related Documents

- Security Incident Response Policy (IR-Policy-Template.md)
- Security Incident Response Process
- Disaster Recovery Plan
- Business Continuity Plan
- Vulnerability Management Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
