# Business Continuity Plan — Implementation Procedures

> **Companion to:** Business Continuity Plan (Template.md)
> **Purpose:** These procedures describe how to implement the continuity requirements defined in the BCP policy. For IT-specific technical recovery, refer to the Disaster Recovery Plan and Disaster Recovery Process.

## Procedure: Work Site Recovery

### Standard Approach

When a facility becomes non-functional due to a disaster:

1. **Declare the work site unavailable.** The ____ (e.g., Facilities and Personnel Safety Team Lead) confirms the site status and notifies the Executive Coordination Team.
2. **Activate remote work protocol:**
   - Send an organization-wide communication via ____ (messaging platform, email, SMS) instructing all personnel at the affected site to work from home or from a pre-designated secondary location.
   - Verify that all personnel have been reached within ____ hours. Use the HR contact roster; call personnel who haven't acknowledged the message.
3. **Assess remote work capability:**
   - Confirm that essential personnel have functioning internet, VPN access, and required hardware. If their issued laptop is at the affected site and unrecoverable, ship a replacement or direct them to the secondary location.
   - Verify that critical SaaS platforms are accessible from remote locations (not IP-restricted to the office).
4. **For roles that cannot function remotely** (e.g., secure lab work, physical asset handling):
   - The ____ team coordinates temporary office space or arranges relocation to a secondary site.
   - Identify a coworking space, partner office, or pre-arranged alternate facility.
   - Provision necessary equipment (laptops, monitors, secure network access) within ____ hours.
5. **Maintain operations:** Critical business processes must continue without dependency on physical presence at any single facility. Validate that payroll, customer support, and revenue operations are functioning within one business day.

### Alternative Approaches

> **💡 Why you might choose differently:** Not all organizations can sustain full remote operations for extended periods.

- **Alternative A — Hot site:** Maintain a pre-provisioned alternate office with desks, network, and equipment ready to go. Higher cost but near-zero transition time for on-site-dependent roles.
- **Alternative B — Dispersed team model:** Design teams to be distributed across multiple offices by default, so no single office loss affects more than a fraction of any team. Business continues with reduced capacity rather than full failover.
- **Alternative C — Mutual aid agreement:** Pre-arrange with a partner organization to share office space in an emergency. Requires a legal agreement and compatibility testing.

### Common Pitfalls

> **⚠️ Watch out:** If your VPN concentrator is in the same physical facility that's now inaccessible, remote work won't work either. Ensure VPN infrastructure is cloud-hosted or at a separate location.
>
> **⚠️ Watch out:** The "everyone work from home" plan fails if home internet is also down (common in regional disasters like earthquakes or hurricanes). Have a plan for personnel in the affected area to relocate outside the disaster zone.
>
> **⚠️ Watch out:** If your identity provider (SSO) is unreachable, personnel can't authenticate to anything. Test that break-glass accounts and offline recovery procedures work when SSO is down.

---

## Procedure: Critical Service Recovery

### Standard Approach

For disruptions affecting customer-facing or revenue-generating services:

1. **Declare the critical service disruption.** The IT and Infrastructure Recovery Team Lead confirms the disruption and notifies the Executive Coordination Team.
2. **Activate the Communications Team:**
   - Update the public status page with: what is affected, what users will experience, estimated time to resolution (if known), and where to find updates.
   - If the status page platform is also affected, use a status-page provider hosted on separate infrastructure (e.g., an independent SaaS status page service).
   - Post updates at least every ____ minutes until resolved.
3. **Execute technical recovery** per the Disaster Recovery Plan:
   - The IT Recovery Team follows the DR Plan's Recovery Phase sequence.
   - If the DR Plan has been activated, coordinate with the DR Coordinator.
4. **Maintain customer support:**
   - If the primary support platform is affected, redirect to a fallback channel (email alias, phone line, alternative ticketing system).
   - Empower support staff to provide proactive communications to affected customers if SLAs are at risk.
   - Document all customer communications for post-event review.
5. **Validate service restoration:**
   - Before declaring the service recovered, run a smoke test: can users log in, access their data, and perform the primary business function?
   - Monitor for ____ hours post-restoration to confirm stability.

### Alternative Approaches

> **💡 Why you might choose differently:** Full DR activation is a heavy process; for short disruptions, a lighter touch may be appropriate.

- **Alternative A — Degraded mode operation:** If full recovery is delayed, bring up a read-only or reduced-capability version of the service so customers have partial access while full recovery proceeds. Communicate clearly what features are limited.
- **Alternative B — Manual workaround:** For some B2B services, a manual process (e.g., processing customer requests via email with a person in the loop) can bridge a short outage without full DR activation.

### Common Pitfalls

> **⚠️ Watch out:** The status page is often hosted on the same infrastructure as the main service. When the service goes down, so does the status page. Always use an independently hosted status-page provider.
>
> **⚠️ Watch out:** "We'll update the status page" means nothing if the person who knows the status-page credentials is unreachable. Pre-share credentials with at least three people across different teams.

---

## Procedure: Third-Party Dependency Failure

### Standard Approach

For disruptions caused by a critical vendor or service provider outage:

1. **Detect the dependency failure.** This may present as a service degradation, error spike, or direct notification from the vendor's status page.
2. **Identify the affected business functions.** Which internal services and processes depend on the failed vendor? Map the blast radius.
3. **Consult the vendor contingency register.** This document (maintained separately, referenced by the BCP) should list for each critical vendor:
   - Alternative providers (pre-vetted, with accounts created and tested)
   - Manual workaround procedures (documented and tested)
   - Contractual SLA and escalation contacts
4. **Evaluate options against RTO:**
   - If the alternative provider can be activated within the affected function's RTO, execute the failover.
   - If the manual workaround can sustain operations within the RTO, activate it.
   - If neither can meet RTO, escalate to executive leadership for a business decision.
5. **Escalate to the vendor:**
   - Open a support ticket immediately.
   - If the vendor provides a premium support tier, use it.
   - Document all vendor communications (timestamps, ticket numbers, what was promised).
6. **Communicate internally and externally:**
   - Notify affected business units that a vendor dependency is down and what the contingency plan is.
   - If customer-facing impact is significant, update the status page.

### Alternative Approaches

> **💡 Why you might choose differently:** Maintaining a full contingency register for every vendor is operationally expensive.

- **Alternative A — Tiered contingency planning:** Only maintain detailed failover procedures for Tier-1 vendors (those whose failure would violate RTOs). For Tier-2 vendors, document only an escalation path.
- **Alternative B — Multi-vendor architecture:** Design critical services to be vendor-agnostic from the start (e.g., abstract the email sending service behind an interface that supports multiple providers). Failover is built into the architecture, not a manual process.
- **Alternative C — Insurance-only approach:** For vendors where the business impact of prolonged outage is primarily financial and below a threshold, rely on business interruption insurance rather than technical failover.

### Common Pitfalls

> **⚠️ Watch out:** You discover the alternative provider's API has changed since you last tested it — and the failover fails. Test vendor failover paths at least annually.
>
> **⚠️ Watch out:** Your "alternative provider" is actually a reseller or subsidiary of the same parent company as your primary provider. When the parent has an outage, both are down. Verify true independence.

---

## Procedure: Annual BCP Testing

### Standard Approach

#### Tabletop Exercise

1. **Plan the exercise** at least ____ weeks in advance:
   - Select a realistic disruption scenario (e.g., ransomware encrypts all file servers, headquarters inaccessible due to earthquake, critical SaaS vendor suffers week-long outage).
   - Define clear objectives (e.g., test notification cascade, test succession chain, test decision-making under time pressure).
   - Identify participants — all response team leads must attend.
   - Prepare a facilitator guide with injects (simulated events) to introduce at specific times during the exercise.
2. **Conduct the exercise:**
   - Facilitator presents the opening scenario.
   - Participants respond as they would in a real event, discussing what actions they'd take, who they'd contact, and what decisions they'd make.
   - Facilitator introduces injects to test edge cases (e.g., "The CEO is unreachable," "The backup restore is corrupted," "A reporter calls asking for comment").
   - Capture all actions, decisions, and discussion points in real time.
3. **Debrief immediately after** (hot wash):
   - What worked as expected?
   - Where did the plan break down?
   - What assumptions were wrong?
   - What information was missing that people needed?
4. **Document the exercise:**
   - Produce a written report within ____ business days.
   - Include: scenario, participants, timeline of actions, what worked, gaps identified, remediation items.
   - Assign each remediation item an owner and a due date.

#### Functional / Technical Test

1. **Select the recovery procedure to test** based on risk (what would hurt most if it failed during a real event) and freshness (what hasn't been tested recently).
2. **Define success criteria:** What does "the test passed" look like? Be specific (e.g., "Application X is accessible at the failover URL and passes the smoke test suite within 45 minutes of initiating failover").
3. **Schedule during a maintenance window** if the test has any risk of production impact.
4. **Execute the test** under observation — capture timestamps for every major step.
5. **Document results:**
   - Success criteria met / not met
   - Actual recovery time vs. RTO target
   - Issues encountered
   - Remediation items

### Common Pitfalls

> **⚠️ Watch out:** Tabletop exercises where everyone agrees "yes, we'd do that" without actually doing it discover almost nothing. Design injects that force hard decisions (budget tradeoffs, priority conflicts).
>
> **⚠️ Watch out:** Annual testing that always uses the same scenario ("primary data center is destroyed") trains the team for only one type of disaster. Rotate scenarios — include cyber attack, vendor failure, pandemic, and insider threat.
>
> **⚠️ Watch out:** Functional tests that succeed because the test environment is simpler than production don't validate real-world recovery. Add complexity each year (more services, more data, cross-service dependencies).

---

## Procedure: BCP Plan Maintenance

### Standard Approach

1. **Schedule the annual review** as a recurring event tied to the annual test cycle. Review the plan AFTER the test so lessons learned are incorporated.
2. **Review checklist:**
   - Are all response team roles filled with current personnel? Update names and contact information.
   - Are RTO/RPO targets still accurate given changes in business operations?
   - Have any new critical business functions been added? Do they need continuity procedures?
   - Have vendors changed? Update the vendor contingency register references.
   - Has the IT architecture changed? Ensure the DR Plan references are still correct.
   - Are regulatory notification requirements still current?
3. **Update after material organizational changes:**
   - Office relocation: within ____ days, update work site recovery procedures and test remote access from the new location.
   - Acquisition or new product launch: assess whether the new business unit or product has continuity requirements. Add to the BCP if critical.
   - Significant IT architecture change: confirm the DR Plan has been updated and cross-reference is correct.
4. **Distribute updated copies** to all response team leads and ensure they maintain local copies.

### Common Pitfalls

> **⚠️ Watch out:** The BCP gets updated but nobody actually reads the new version. After each update, conduct a 30-minute walkthrough with team leads to confirm they understand the changes.
>
> **⚠️ Watch out:** Contact information rots fast. The person listed as "Facilities Lead" left six months ago. Validate the contact roster quarterly, not just at the annual review.

## Related Documents

- Business Continuity Plan (Template.md)
- Disaster Recovery Plan
- Disaster Recovery Process
- Backup Policy
- Incident Response Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
