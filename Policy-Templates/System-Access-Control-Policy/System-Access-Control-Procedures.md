# System Access Control Policy — Implementation Procedures

> **Companion to:** System Access Control Policy (Template.md)
> **Purpose:** These procedures describe how to implement the requirements set forth in the System Access Control Policy. The policy defines WHAT must be done; this document describes HOW to do it.

---

## Procedure 1: Access Provisioning

### Standard Approach

This procedure covers the end-to-end process for granting access to organizational systems and data.

#### Step 1: Request Submission

1. The requestor (typically the individual's manager, or the individual themselves if self-service is enabled) submits an access request through `____` (approved ticketing system / IGA platform, e.g., ServiceNow, Jira Service Management, Okta Access Requests, SailPoint).
2. The request form must capture:
   - **Individual's Identity:** Full name, employee/contractor ID, department, manager.
   - **Systems Requested:** Specific application, server, database, or network resource. Use system names from the CMDB.
   - **Access Level:** Role-based (e.g., "Salesforce — Sales User") or specific permissions (viewer, editor, admin).
   - **Business Justification:** Free-text describing why this access is needed for job responsibilities.
   - **Duration:** Permanent (ongoing role) or temporary (end date: `____`).
3. The ticketing system auto-routes the request to the appropriate approver(s) based on system and access level.

#### Step 2: Approval

1. **Standard Access:** System owner or designated approver reviews the request for appropriateness, least privilege alignment, and separation of duties conflicts. Approval or denial within `____` business days.
2. **Privileged Access:** Additional approval from `____` (Security Officer / CISO) is required. The approver verifies that the individual's role genuinely requires elevated privileges and that compensating controls (session logging, JIT) will be applied.
3. **Third-Party / Contractor Access:** Additional approval from `____` (Vendor Manager / Procurement) confirming a valid contract with security obligations is in place.
4. If denied, the approver provides a reason. The requestor may resubmit with additional justification.

#### Step 3: Identity Verification

1. For new hires: identity is verified during onboarding by HR (government ID check, background verification). No additional verification needed if access request aligns with the individual's role.
2. For existing personnel requesting NEW access (role change or additional systems): the approver confirms the request is consistent with the individual's role. Out-of-band verification (phone call, Slack DM, video call) is required for sensitive access grants.
3. For remote personnel: out-of-band verification is mandatory — do not approve based solely on an email or ticket.

#### Step 4: Provisioning

1. The IAM team or automated provisioning system creates the account with permissions matching the approved role.
2. For RBAC: assign the individual to the approved role group(s). Do NOT grant individual custom permissions unless the role does not exist and a temporary exception is approved.
3. For privileged access: provision via the PAM/JIT access broker (`____`, e.g., CyberArk, Delinea, Teleport). Set automatic expiration matching the approved duration.
4. Provisioning must be completed within `____` business hours of approval.

#### Step 5: Acknowledgement and Activation

1. Before access is activated, the individual must:
   - Acknowledge the System Access Control Policy, Acceptable Use Policy, and any system-specific security requirements.
   - Confirm completion of required security awareness training (if not already current).
2. Acknowledgement is captured digitally via `____` (IGA platform, policy management tool).
3. After acknowledgement, access is activated. The individual receives credentials via a secure channel (NOT email — use password manager sharing, one-time link, or in-person handoff).
4. On first login, the individual is forced to set a new password (if password-based) or complete MFA enrollment.

#### Post-Provisioning Validation

1. Within `____` business days, the provisioning team verifies that the account has the correct permissions and no unintended access.
2. The manager confirms the individual can perform their required job functions with the granted access.
3. The access request is closed in the ticketing system with provisioning details recorded.

### Alternative Approaches

> **💡 Why you might choose differently:** Organizational size, tooling maturity, and compliance requirements dictate the level of automation and rigor.

- **Fully Automated Birthright Provisioning:** When an HR system creates a new employee record, the IGA platform automatically provisions baseline access based on department, role, and location — no manual request or approval needed. Exception access still requires request/approval. Reduces time-to-productivity but requires mature role definitions.
- **Self-Service with Auto-Approval:** For low-risk applications (e.g., corporate wiki, internal chat), allow self-service access requests that are auto-approved if no separation-of-duties conflict is detected. Speeds up access for commodity tools.
- **Manager-Only Access Control:** The individual's manager is solely responsible for requesting access. The IAM team provisions exactly what the manager requests after confirming no SoD conflicts. Simpler chain of custody; relies heavily on manager diligence.

### Common Pitfalls

> **⚠️ Watch out:** Access creep. An individual changes roles 3 times over 5 years and accumulates access from every previous role. At provisioning for a NEW role, always review and remove old role access. Role changes are not additive by default.

> **⚠️ Watch out:** "Just give them the same access as [coworker]." Copying access from a peer is the #1 source of privilege creep. Every access grant must be based on role requirements, not another individual's potentially over-provisioned access.

> **⚠️ Watch out:** Delayed provisioning. If it takes 2 weeks to get a developer access to the CI/CD pipeline, they'll find workarounds (shared credentials, bypass). Track provisioning SLAs as a KPI.

> **⚠️ Watch out:** Failing to verify identity for remote workers. Social engineering attackers exploit the access request process by impersonating employees. Always use out-of-band verification for sensitive access changes.

---

## Procedure 2: Access Reviews

### Standard Approach

#### 2.1 Review Initiation

1. The Security Officer (or delegate) initiates the access review on a `____` (quarterly) schedule.
2. The review is managed via the IGA platform (`____`) or, for organizations without an IGA platform, via a structured spreadsheet distributed through the ticketing system.
3. The review scope includes:
   - All user accounts and their associated permissions across all in-scope systems.
   - Privileged accounts (reviewed `____` (monthly), more frequently than standard accounts).
   - Shared/generic accounts (reviewed `____` (monthly)).
   - Third-party and contractor accounts (reviewed `____` (monthly)).
   - Service accounts (reviewed `____` (quarterly) — confirm they are still in use by an active service).

#### 2.2 Reviewer Assignment

1. System owners and managers receive their assigned review population: the individuals and accounts under their purview.
2. Reviewers are notified via email with a link to the review interface. The notification includes: review deadline (`____` days from initiation), review instructions, and escalation path for questions.

#### 2.3 Review Execution

For each access entitlement, the reviewer must confirm:
- **Continued Need:** Does the individual still require this access for their current job responsibilities?
- **Appropriate Level:** Is the level of access (read, write, admin) appropriate? Could it be reduced?
- **No Unauthorized Escalation:** Has the individual's access changed since the last review? If so, was the change approved?
- **Separation of Duties:** Does the individual have conflicting access that should be segregated?

The reviewer certifies each entitlement as:
- **Approve:** Access is appropriate; no changes needed.
- **Modify:** Access should be reduced or changed.
- **Revoke:** Access is no longer needed and must be removed.
- **Flag for Investigation:** Something looks unusual; Security team should investigate before certifying.

#### 2.4 Remediation

1. At the review deadline, the Security Officer compiles all "Revoke" and "Modify" decisions.
2. The IAM team implements changes within `____` business days.
3. Any "Flag for Investigation" items are assigned to the Security team for investigation within `____` business days.
4. Reviewers who fail to complete their review by the deadline are escalated: first reminder at `____` days before deadline, manager escalation at deadline, executive escalation at `____` days past.

#### 2.5 Documentation and Evidence

1. The completed review (all certifications, modifications, revocations) is exported as a dated, immutable record.
2. Retain review records for `____` years.
3. A summary report is presented to management: total accounts reviewed, accounts modified/revoked, privileged accounts reviewed, overdue reviewers, and key findings.

### Alternative Approaches

> **💡 Why you might choose differently:** Review frequency, depth, and tooling depend on organizational size and risk profile.

- **Continuous Certification (Zero-Trust Approach):** Instead of quarterly point-in-time reviews, implement continuous certification where access is automatically validated against HR data and flagged instantly on discrepancies. A quarterly review becomes a validation of the continuous process, not the primary control.
- **Manager-Led vs. System-Owner-Led:** Some organizations route all access reviews through managers ("Does your team member need this?"). Others route through system owners ("Does this person need access to MY system?"). Best practice: both — managers certify need, system owners certify appropriateness of level.
- **Campaign-Based Review for Large Orgs:** For organizations with >1000 employees, stagger reviews by department on a rolling quarterly schedule rather than reviewing everyone at once. Reduces reviewer fatigue.

### Common Pitfalls

> **⚠️ Watch out:** Rubber-stamping. Reviewers clicking "Approve All" without actually reviewing. Mitigate by: limiting review population per reviewer, requiring justification for every "Approve," and implementing spot-check audits of reviewer decisions.

> **⚠️ Watch out:** The "ghost account" problem. Accounts for terminated employees that weren't properly deprovisioned appear in the review as active accounts with no manager to certify them. Flag any account with no assigned manager for immediate investigation.

> **⚠️ Watch out:** Reviewing the wrong thing. A review that certifies "User X has Role Y" doesn't catch that Role Y grants access to System Z, which Role Y shouldn't include. Review at the entitlement level, not just the role level. Or, review role composition AND role membership.

---

## Procedure 3: Offboarding and Access Revocation

### Standard Approach

#### 3.1 Voluntary Departure (Resignation, Retirement)

1. **HR Notification:** HR notifies the IAM team and Security Officer of the departure at least `____` business days before the last working day, OR immediately upon notification from the employee if shorter notice.
2. **Pre-Departure Access Review:** The IAM team compiles a list of all system accounts, physical access cards, and assets assigned to the individual. This list is shared with the manager for completeness verification.
3. **Physical Access Revocation:** Building access cards, office keys, and parking passes are collected on the last working day. Building access is disabled in the access control system by end of day.
4. **Logical Access Revocation:** All system accounts are disabled within `____` business day of the last working day. This includes:
   - Primary user account (SSO/IdP).
   - Application-specific accounts.
   - VPN and remote access.
   - Email account (set to auto-responder: "_____ is no longer with _____; please contact _____").
   - Cloud service accounts (AWS IAM, Azure AD, GCP IAM).
   - Third-party SaaS tools managed through SSO (revoking IdP access cascades to most tools).
5. **Asset Return:** The termination checklist tracks return of all organization-owned equipment (laptop, phone, security tokens, external drives, documents). Items are signed back in by IT/Facilities.
6. **Data Handling:** The individual's data (email, files) is preserved per legal hold requirements. After the retention period (`____` days/weeks), data is securely deleted per the Data Protection Procedures.

#### 3.2 Involuntary Termination

1. **Pre-Coordination:** HR and Security coordinate the termination timing. Logical access may need to be revoked BEFORE the individual is notified, depending on risk assessment.
2. **Immediate Access Revocation:** ALL access is revoked simultaneously with (or immediately before) the termination meeting:
   - Active sessions are force-terminated.
   - Accounts are disabled.
   - VPN tokens are revoked.
   - Building access is disabled.
   - Mobile device management (MDM) issues a remote wipe command if the device contains sensitive data and cannot be physically recovered.
3. **Physical Security:** Security personnel are available if there is any risk of confrontation or if the individual needs to be escorted from the premises.
4. **Post-Termination Monitoring:** Monitor the individual's accounts and email forwarding rules for `____` days post-termination to detect any unauthorized access attempts or backdoor activity.

#### 3.3 Role Change (Internal Transfer)

1. **Manager Notification:** The individual's current manager notifies the IAM team of the role change effective date.
2. **Previous Role Access Review:** The IAM team compiles all access from the previous role. The previous manager certifies which access should be removed.
3. **Access Removal and Provisioning:** Previous role access is removed effective on the role change date. New role access is provisioned per the standard provisioning procedure (Procedure 1).
4. **"Clean Slate" Principle:** Access should not accumulate across roles. If an individual transfers from Finance to Engineering, their Finance system access is removed (unless explicitly justified and approved by both department heads).

#### 3.4 Extended Inactivity

1. **Detection:** Automated systems monitor last-login timestamps across all accounts.
2. **Disablement:** Accounts inactive for `____` days are automatically disabled. The individual and their manager are notified.
3. **Deletion:** Disabled accounts that remain unused for an additional `____` days are permanently deleted. Data associated with the account is handled per the Data Retention Policy.
4. **Exception Handling:** Exceptions (e.g., long-term leave, sabbatical, maternity/paternity leave) must be documented in the HR system BEFORE the inactivity threshold triggers. HR notifies IAM to place the account in "leave" status instead of "inactive."

#### 3.5 Verification and Closure

1. The offboarding process is tracked via a termination checklist in `____` (ticketing system / IGA workflow).
2. Every item on the checklist must be marked complete before the ticket can be closed.
3. A post-offboarding audit is conducted `____` days after offboarding: attempt to authenticate with the terminated individual's credentials. Any successful authentication triggers an immediate investigation.

### Alternative Approaches

> **💡 Why you might choose differently:** Organizational size, geographic distribution, and termination volume influence the right approach.

- **HRIS-Triggered Automation:** The HR system (e.g., Workday, BambooHR) fires a webhook on employment status change → the IAM platform automatically disables all accounts → the ticketing system creates a termination ticket pre-populated with the offboarding checklist. Zero manual initiation. Requires mature systems integration.
- **Identity Provider (IdP)-Centric Revocation:** For organizations where 90%+ of applications are behind SSO, disabling the IdP account effectively revokes access to everything. Non-SSO systems require manual cleanup, but the bulk of revocation is instantaneous. Simplifies the process dramatically.
- **Physical Access First, Logical Second:** For geographically distributed companies, coordinate so that the physical office badge is deactivated, but logical access may remain active for a transition period (only for voluntary departures). Not applicable for involuntary terminations.

### Common Pitfalls

> **⚠️ Watch out:** Forgetting about non-SSO applications. The Okta account is disabled, but the individual also had a direct login to the legacy database, a local account on the jump server, and a shared API key. Maintain an inventory of ALL access points, not just SSO-managed ones.

> **⚠️ Watch out:** Email account kept active "to forward to the replacement." This is a huge data protection risk. Set an auto-responder directing senders to the new contact. Do NOT forward the departed employee's email indefinitely — it may contain sensitive data the replacement shouldn't access.

> **⚠️ Watch out:** Service accounts and API keys created by the individual. If a departing DevOps engineer created AWS IAM access keys for a script, simply disabling their user account doesn't revoke those keys. Audit for user-created long-lived credentials as part of the offboarding checklist.

> **⚠️ Watch out:** Delayed notification from HR. If HR notifies IT of a termination 3 days after it happened, the individual had 3 days of access they shouldn't have had. Integrate HR and IAM systems to eliminate this gap, or implement an HR SLA for termination notification (e.g., within 1 business hour).

---

## Related Documents

- System Access Control Policy (Template.md)
- Information Security Policy (ISP-Template.md)
- Password Policy
- Acceptable Use Policy (AUP-Template.md)
- Data Protection Policy
- Vendor Management Policy
