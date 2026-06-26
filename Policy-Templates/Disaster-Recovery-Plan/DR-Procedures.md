# Disaster Recovery Plan — Implementation Procedures

> **Companion to:** Disaster Recovery Plan (Template.md)
> **Purpose:** These procedures describe how to execute the three DR phases defined in the policy. They focus on the phase-level execution steps. For platform-specific failover procedures (CRM, financial systems, communication tools), refer to the Disaster Recovery Process document.

## Procedure: Notification and Activation (Phase 1)

### Standard Approach

#### Detection and Initial Response

1. **The first responder detects the disruption.** This may be:
   - An automated alert from monitoring indicating multiple critical systems are down.
   - A manual report from a team member who cannot access critical services.
   - A cloud provider status page notification of a regional outage.
   - A physical event (fire, flood, power loss) at a facility hosting infrastructure.
2. **Notify immediately:** The first responder contacts ____ (e.g., CTO / Head of Engineering) via all available channels — messaging platform, SMS, and phone. Provide:
   - What was observed (specific systems, error messages, or physical conditions).
   - When it started.
   - Any initial assessment of scope (single system, entire region, unknown).
3. **The ____ (DR Authority) acknowledges** and assumes incident command. If unreachable within ____ minutes, the succession chain activates (next in line assumes authority).

#### Damage Assessment

1. **Assemble the assessment team:**
   - Infrastructure Lead (or equivalent) to assess technical systems.
   - Security Officer to assess security implications.
   - If physical damage is involved, Facilities Lead.
2. **Determine the nature and extent of damage:**
   - **Infrastructure salvageable?** Can affected systems be restored in place, or is a full rebuild required?
   - **Data integrity?** Are backups intact and recent enough to meet RPO?
   - **Estimated recovery time?** Under best-case, expected, and worst-case scenarios.
   - **Safety concerns?** Are personnel at risk? Is the facility safe?
   - **Security state?** Was the disruption caused by an attack? Is the attacker still present?
3. **Formulate an initial recovery approach:**
   - Recover in place (if infrastructure is salvageable)
   - Fail over to alternate site (if primary infrastructure is unrecoverable)
   - Hybrid (recover some systems in place, fail over others)
4. **Document the assessment** in the event log with timestamps.

#### Activation Decision

The DR Authority activates the DR Plan if **any** of these criteria are met:

- Critical systems will be unavailable for more than ____ hours (from the RTO table in the policy).
- A hosting facility or cloud region is damaged and will be unavailable for more than ____ hours.
- A cybersecurity incident has caused widespread data destruction or system compromise.
- Executive leadership declares activation for any other reason.

#### Activation Actions (Execute Immediately Upon Activation)

1. **Notify all recovery team members:**
   - Send activation notice via messaging platform + SMS + email.
   - Include: event description, activation time, immediate action required from each team.
   - Confirm receipt from every team member; call anyone who doesn't acknowledge within ____ minutes.
2. **Notify external partners:**
   - Cloud provider / hosting provider: open a high-severity support case.
   - Critical vendors that may be affected or can assist.
   - Cyber insurance provider (if applicable).
3. **Notify executive leadership** with current status, estimated recovery timeline, and any resource needs.
4. **Activate the communications plan:**
   - Update public status page.
   - Prepare customer notification if required by SLA or regulation.
   - Designate a single point of contact for all external communications.
5. **Start the event log:** Record every decision, action, and communication with timestamps. This log becomes the authoritative record of the DR event.

### Alternative Approaches

> **💡 Why you might choose differently:** Full activation is noisy and disruptive. For contained incidents, a partial activation may be appropriate.

- **Alternative A — Phased activation:** Activate only the Recovery Phase for affected systems without declaring a full DR event. Use this when systems are down but the recovery path is clear and contained (e.g., a single database failure with a known-good backup). Does not trigger external notification.
- **Alternative B — Pre-authorized auto-activation:** For cloud region outages detected by automated monitoring, pre-authorize the Infrastructure Lead to activate the DR Plan and begin failover without waiting for executive approval. Requires clear guardrails (which scenarios, what spend limits).

### Common Pitfalls

> **⚠️ Watch out:** The notification cascade assumes the messaging platform is working. If it's part of the affected infrastructure, you're trying to coordinate in the dark. Pre-distribute a phone tree and SMS list that doesn't depend on any internal system.
>
> **⚠️ Watch out:** "We'll notify customers when we know more" leads to radio silence for hours while the team assesses damage. Customers would rather hear "we're investigating, we'll update you in 30 minutes" than silence. Commit to a communication cadence, not a content threshold.
>
> **⚠️ Watch out:** Activation is often delayed because the DR Authority wants "more information" before committing. The policy's activation criteria exist precisely to prevent this — if the criteria are met, activate. You can always stand down later.

---

## Procedure: Recovery (Phase 2)

### Standard Approach

Execute the following sequence. Steps may run in parallel where dependencies permit:

#### Step 1: Communicate

- Send initial customer notification (if not already done during Activation).
- Provide the first timeline estimate (even if rough: "we estimate 4-8 hours").
- Commit to next update time: "We will provide our next update at ____."

#### Step 2: Assess

- Complete the damage assessment (if not completed during Phase 1).
- Confirm the recovery approach: in-place recovery, alternate site failover, or hybrid.
- Confirm which backups will be used for restoration (most recent verified backup that predates the disruption).

#### Step 3: Provision

- Deploy the recovery environment at the alternate site using infrastructure-as-code (IaC).
- **If using a secondary cloud region:** Execute the IaC deployment against the failover region. Validate that networking, security groups, and IAM roles resolve correctly in the new region context.
- **If using a secondary cloud provider:** Execute the multi-cloud IaC deployment. Validate that provider-specific resources (e.g., managed database engines, load balancer types) have been correctly mapped.
- **If using on-premises:** Power on the recovery hardware, deploy from the configuration management system.
- Provisioning should target a production-equivalent environment — same instance sizes, same database tiers, same network topology.

#### Step 4: Restore

- Restore databases from the most recent verified backups:
  - Verify backup integrity before restoring (checksums, file sizes).
  - Restore in priority order: databases supporting critical systems first.
  - Execute point-in-time recovery if transaction log backups are available and needed.
- Restore file/object storage.
- Restore configuration data (secrets, certificates, environment variables) from secure backup stores.

#### Step 5: Validate

- **Smoke tests:** Can the application start? Can users authenticate? Can core business operations execute?
- **Integration tests:** Do services connect to each other correctly? Do third-party integrations work?
- **Data validation:** Are row counts, checksums, and recent transactions consistent with expectations?
- **Performance check:** Is the recovery environment performing adequately? (It may be degraded from production — document any degradation.)

#### Step 6: Secure

- Confirm encryption is enabled on all data stores.
- Confirm logging is flowing to the centralized log platform.
- Confirm monitoring alerts are configured and firing correctly.
- Confirm access controls are in place (security groups, IAM policies, network ACLs).
- Run a vulnerability scan on the recovery environment before exposing it to users.

#### Step 7: Patch

- Apply any outstanding security patches that the backup may not include.
- Update any certificates that may have expired since the backup was taken.

#### Step 8: Activate

- Designate the recovery environment as the active production environment.
- Update internal references (DNS, service discovery, configuration) to point to the recovery environment.

#### Step 9: Route

- Update public DNS records to direct users to the recovery environment.
- Monitor DNS propagation (it can take hours for some users).
- If using a CDN, update origin server configuration.
- Monitor traffic, errors, and user reports for the first ____ hours after cutover.

### Alternative Approaches

> **💡 Why you might choose differently:** The full 9-step sequence assumes a complete rebuild. Some scenarios call for lighter approaches.

- **Alternative A — Pilot light recovery:** Maintain a minimal recovery environment running at all times (e.g., a small database replica and one app server). On activation, scale up rather than provision from scratch. Faster recovery (minutes instead of hours) at higher ongoing cost.
- **Alternative B — Warm standby:** Maintain a fully-provisioned but scaled-down copy of production in the failover region. Data is continuously replicated. On activation, scale up and cut over. Best RTO but highest cost.
- **Alternative C — Backup-only recovery:** For non-critical systems, skip steps 3 (Provision) and 8-9 (Activate/Route) by restoring directly to the original environment once it's available. Accept longer downtime in exchange for operational simplicity.

### Common Pitfalls

> **⚠️ Watch out:** IaC that works in us-east-1 may fail in eu-west-1 because of regional service availability differences (e.g., a specific instance type or managed service tier isn't offered in the failover region). Test IaC deployment in the failover region BEFORE you need it.
>
> **⚠️ Watch out:** DNS changes have TTL. If your TTL is 24 hours, some users won't see the cutover for a day. Lower TTLs before planned maintenance; for unplanned DR, accept that routing will be gradual.
>
> **⚠️ Watch out:** The recovery environment will have different IP addresses. Hardcoded IPs in application config, third-party allowlists, or customer firewall rules will break. Document all IP-dependent integrations and have a procedure to update them.

---

## Procedure: Reconstitution (Phase 3)

### Standard Approach

Once the original site is restored or a new permanent site is established:

1. **Provision the permanent environment:**
   - Deploy production infrastructure using the same IaC templates that were validated during Recovery.
   - If using a new permanent site (not the original), this is a greenfield deployment.
   - If the original site has been repaired, ensure it is clean and secure before provisioning.
2. **Replicate data:**
   - Set up continuous replication from the recovery environment to the permanent environment (if supported by the database engine).
   - Alternatively, take a fresh backup from the recovery environment and restore to the permanent environment.
   - Validate data consistency after replication.
3. **Execute validation tests** against the permanent environment:
   - Same smoke tests, integration tests, and data validation as Recovery Phase Step 5.
   - Add a full regression test suite if time permits.
4. **Verify security controls:**
   - Logging, monitoring, and alerting operational.
   - Encryption enabled.
   - Access controls correct.
   - Vulnerability scan clean.
5. **Plan the cutover:**
   - Choose a low-traffic window if possible.
   - Lower DNS TTLs in advance.
   - Brief all teams on the cutover plan and rollback procedure.
6. **Execute the cutover:**
   - Deploy the permanent environment as production.
   - Update DNS to direct traffic to the permanent environment.
   - Monitor for ____ hours post-cutover.
7. **Decommission the recovery environment:**
   - After confirming stability, begin decommissioning the recovery environment.
   - Sanitize all storage (secure wipe or cryptographic erasure).
   - Terminate all compute resources.
   - Document decommissioning actions.

### Alternative Approaches

> **💡 Why you might choose differently:** Reconstitution adds risk — you're moving production a second time. Sometimes staying put is better.

- **Alternative A — Stay at recovery site:** If the recovery environment is stable, equivalent in capability, and the original site cannot be restored cost-effectively, designate the recovery environment as the new permanent production site. Rebuild redundancy from there.
- **Alternative B — Gradual migration:** Move services back to the permanent site one at a time over days or weeks, rather than a single big-bang cutover. Lower risk per service but prolonged co-existence complexity.

### Common Pitfalls

> **⚠️ Watch out:** Reconstitution is often treated as an afterthought — "we'll figure it out after recovery." But it needs the same level of planning as the initial failover. An unplanned cutback to the original site can cause a second outage.
>
> **⚠️ Watch out:** The recovery environment may have accumulated configuration drift (emergency patches, workarounds, hotfixes) that aren't in the IaC templates. If you redeploy from IaC at the permanent site, those fixes are lost. Audit the recovery environment's config before reconstitution.

---

## Procedure: DR Plan Deactivation and Post-Event Review

### Standard Approach

1. **Declare deactivation:** Once the permanent environment is stable for ____ hours, the DR Authority formally declares the DR Plan deactivated. Notify all recovery team members and executive leadership.
2. **Compile the event record:**
   - Gather all event logs, incident tickets, communication records, and decision logs.
   - Organize into a chronological timeline.
   - Identify key decision points and who made them.
3. **Conduct post-incident review** within ____ business days:
   - All recovery team members participate.
   - Review the timeline — what happened when?
   - Evaluate: what worked, what didn't, what was missing?
   - Measure actual recovery times against RTO/RPO targets.
   - Identify procedural gaps, tooling deficiencies, and training needs.
4. **Update all DR documentation:**
   - Update the DR Plan with lessons learned.
   - Update the DR Process with platform-specific findings.
   - Update IaC templates if they failed in the failover region.
   - Update contact rosters if any were inaccurate.
5. **Track remediation items** in the ticketing system. Assign owners and due dates.

### Common Pitfalls

> **⚠️ Watch out:** Post-incident reviews that happen weeks later suffer from memory decay. Conduct the hot wash within 48 hours of deactivation while details are fresh.
>
> **⚠️ Watch out:** Remediation items from post-incident reviews often go into a black hole. Schedule a 30-day follow-up meeting specifically to review remediation progress.

## Related Documents

- Disaster Recovery Plan (Template.md)
- Disaster Recovery Process
- Business Continuity Plan
- Backup Policy
- Incident Response Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
