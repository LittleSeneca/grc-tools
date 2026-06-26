# Password Policy

Policy Title: Password Policy
Policy Number: ISP-006
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Purpose

This policy establishes the requirements for creating, managing, and protecting authentication credentials used to access ____ systems and data.

## Scope

This policy applies to all Personnel who have accounts on any system that resides at any organizational facility or has access to the organizational network, including employees, contractors, and third-party users.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (Security Officer) | Annual review; policy enforcement |
| ____ (IT/Infrastructure) | Technical enforcement of password requirements |
| All Personnel | Compliance with password requirements |

## Policy

### Password Requirements

In alignment with NIST SP 800-63B Digital Identity Guidelines:

- Passwords must be at least ____ (recommended: 12) characters in length.
- Passwords must not be based on dictionary words, common passwords, or personal information.
- Password complexity requirements (uppercase, lowercase, digits, special characters) should be enforced only when required by specific regulatory obligations. Length-based strength is preferred over complexity rules.
- Passwords must not be reused across distinct services and systems.
- Passwords should not be required to change on a fixed schedule unless there is evidence of compromise. Forced periodic password rotation is discouraged as it leads to weaker password choices (NIST SP 800-63B).
- All passwords should be generated and stored using an approved password manager.
- Default, temporary, and shared accounts are prohibited. Each user must have a unique account.

### Multi-Factor Authentication (MFA)

- MFA must be enabled for all systems that provide MFA capability.
- MFA must use phishing-resistant methods (FIDO2/WebAuthn, hardware security keys) where feasible.
- SMS-based MFA should be avoided where stronger alternatives exist.
- MFA must be required for all remote access, administrative access, and access to systems containing sensitive data.

### Password Storage and Transmission

- Passwords stored in systems must be hashed using an approved algorithm (bcrypt, scrypt, Argon2id, or PBKDF2 with sufficient iterations) with a unique, random salt per credential.
- Passwords must never be stored in plaintext or transmitted over unencrypted channels.
- Passwords must not be displayed on screen during entry or stored in scripts, code repositories, or configuration files.
- Use of environment variables or secrets management services is required for application credentials.

### Password Distribution

- Initial or temporary credentials must be unique, non-guessable, and transmitted to the user through a secure channel.
- The user's identity must be verified before providing new or replacement credentials.
- Temporary credentials must be changed upon first use.
- Credentials must never be transmitted via unencrypted email.

### Password Protection

- All passwords are treated as confidential information and must not be shared with anyone.
- If a password must be shared for operational reasons, use the approved password manager's secure sharing feature or grant access through single sign-on (SSO).
- Passwords must not be written down, stored in emails, electronic notes, or mobile devices outside of the approved password manager.
- If a password is suspected of being compromised, it must be rotated immediately and the ____ (Security Officer) must be notified.

## Enforcement

Violation of this policy may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Information Security Policy (ISP-001)
- System Access Control Policy
- Encryption Policy
- Acceptable Use Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
