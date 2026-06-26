# Password Policy — Implementation Procedures

> **Companion to:** [Password Policy](Template.md)  
> **Purpose:** These procedures describe how to implement the Password Policy requirements. The policy defines WHAT authentication standards apply; this document describes HOW to operationalize password management, distribution, and protection.

---

## Procedure 1: Password Manager Deployment

### Standard Approach

1. **Select and procure.** Choose an enterprise password manager with SSO integration, MFA support, and secure sharing. Approved platform: `____` (e.g., 1Password Business, Bitwarden Enterprise, Keeper).

2. **Integrate with identity provider.** Configure SSO via `____` (e.g., Okta, Entra ID, Google Workspace) so users authenticate with their existing organizational credentials. This eliminates a separate master password for most users.

3. **Provision accounts.** Auto-provision accounts for all Personnel via directory sync. Include contractors and vendors who access organizational systems.

4. **Configure policies:**
   - Minimum master password length: `____` characters (recommended: 14+).
   - Require MFA for vault access.
   - Enable breach monitoring / dark web scanning (e.g., Have I Been Pwned integration).
   - Configure shared vaults for departmental credentials.

5. **Train users.** Provide a 15-minute training session during onboarding covering: generating strong passwords, using the browser extension, secure sharing (never share in Slack/email), and the travel mode feature (if applicable).

6. **Monitor adoption.** Track vault activation rate. Target: `____`% within `____` days of deployment. Follow up individually with non-adopters.

> **💡 Alternative approaches:**
> - **SSO-only approach:** If your identity provider supports passwordless (FIDO2/WebAuthn) and all critical apps are behind SSO, the password manager becomes less important. Invest in SSO coverage instead.
> - **Built-in browser password manager:** Some organizations allow Chrome/Edge password managers for non-privileged users. Lower friction but limited sharing, no admin controls, and harder to revoke on offboarding.
>
> **⚠️ Watch out:**
> - **The password manager is a single point of failure.** Protect the admin console with phishing-resistant MFA (FIDO2 key). If an attacker compromises the password manager admin, they own every credential.
> - **Emergency access.** Designate 1-2 trusted administrators with break-glass access to the password manager in case the primary admin is unavailable. Document and test this process.

---

## Procedure 2: Secure Password Distribution

### Standard Approach

1. **Temporary credential generation.** When a new account is created or a password must be reset, the provisioning system (IdP, password manager, or IT ticketing system) generates a unique, random temporary credential.

2. **Identity verification.** Before providing credentials, verify the user's identity:
   - **In person:** Government-issued ID check by IT or HR.
   - **Remote:** Video call with manager confirmation, or verification through a previously established communication channel (phone number on file in HR system).
   - **Automated:** SSO/IdP self-service password reset with MFA verification.

3. **Secure transmission.** Deliver the temporary credential through a secure channel:
   - Password manager secure sharing link (expires in `____` hours).
   - IdP self-service reset flow (user sets their own password — no transmission needed).
   - Encrypted messaging platform (if available).
   - **Never:** plaintext email, Slack, SMS, or written on paper.

4. **First-use enforcement.** Configure the system to require password change on first login. The temporary credential is single-use.

5. **Audit.** Log all credential distribution events: who requested, who approved, method of verification, method of delivery, and timestamp.

> **💡 Alternative: Eliminate temporary passwords entirely.** With modern SSO + passwordless authentication, many systems can provision accounts without ever generating a password. The user authenticates with their IdP credentials and the system never sees a password. This is the ideal state — fewer passwords = fewer distribution problems.
>
> **⚠️ Watch out:**
> - **Phone verification is weak.** SIM-swapping attacks make phone-based verification unreliable for privileged accounts. For admin accounts, require in-person or video verification.
> - **Temporary password expiry.** If the user doesn't log in within `____` hours, the temporary credential should expire and require re-issuance.

---

## Procedure 3: Credential Compromise Response

### Standard Approach

1. **Detection.** Compromise may be detected through: user report, security tool alert (impossible travel, credential stuffing detection), dark web monitoring (Have I Been Pwned domain alert), or incident response findings.

2. **Immediate containment:**
   - Force password reset on the affected account in the identity provider.
   - Terminate all active sessions for the user.
   - Temporarily revoke privileged access if the account had elevated permissions.

3. **Rotation scope.** Rotate all credentials the user had access to:
   - Personal accounts: force reset on IdP, password manager prompts user to update stored credentials.
   - Shared/service accounts the user knew: rotate immediately by the service account owner.
   - If a password manager vault was potentially compromised: rotate ALL stored credentials.

4. **Investigation.** Determine: how was the credential compromised (phishing, database breach, shoulder-surfing, malware)? Was the credential used to access systems? What data was potentially exposed?

5. **Notification.** Notify `____` (Security Officer). If customer data was involved, follow the Incident Response Policy for breach notification.

6. **Post-incident.** Document lessons learned. If phishing was the vector, update training content. If a specific system was breached, assess whether password hashing was adequate.

> **⚠️ Watch out:**
> - **Don't punish the reporter.** If a user voluntarily reports "I think I fell for a phishing email and entered my password," the response should be "thank you for reporting" + immediate remediation, not disciplinary action. Punishing reporters guarantees nobody reports next time.
> - **Rotating everything takes time.** A full credential rotation across all services for a compromised admin can take hours. Prioritize: external-facing services first, then internal systems, then personal accounts.
