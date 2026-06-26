# Database Password Management Process

Policy Title: Database Password Management Process
Policy Number: ISP-____
Effective Date: ____
Version: 1.0
Classification: Internal
Approved By: ____

## Introduction

In accordance with the Password Policy, all database credentials — particularly those protecting production data — must be unique, complex, and regularly rotated. This document provides the operational procedures for managing and rotating database passwords across all production database systems while maintaining service uptime.

This process applies to all production database systems, including managed database services, self-hosted database instances, and database clusters used by applications and services.

## Credential Storage Architecture

All production database credentials must be stored in a centralized secrets management service. The organization's standard secrets management platform stores credentials as encrypted secrets, with access controlled by identity and access management policies.

### Connection Patterns

Applications and services connect to databases using one of two patterns:

1. **Direct Secrets Retrieval:** The application retrieves database credentials at startup (or on a refresh interval) directly from the secrets management service via API call or SDK integration.

2. **Environment Variable Injection:** Database credentials are injected into the application's runtime environment via an environment file or configuration variable, which is protected by file system permissions, user access controls, and network access controls.

The direct secrets retrieval pattern (Pattern 1) is the preferred approach for production services because it eliminates static credential files, enables automatic rotation without application restart, and centralizes access auditing. The environment variable pattern (Pattern 2) should be migrated to Pattern 1 where technically feasible.

### Current Configuration

A comprehensive inventory of all database connection configurations must be maintained, documenting:

| Application / Service | Database Type | Host | Connection Pattern | Credential Storage Location | Rotation Schedule |
|----------------------|---------------|------|-------------------|-----------------------------|-------------------|
| ____ | ____ | ____ | Direct / Env File | ____ | ____ |
| ____ | ____ | ____ | Direct / Env File | ____ | ____ |

## Password Rotation Schedule

### Automated Rotation

Database credentials stored in the secrets management service that supports automatic rotation must be configured for automatic rotation on the following schedule:

- Production database credentials: Rotate every ____ days (recommended: 90 days).
- Non-production database credentials: Rotate every ____ days (recommended: 180 days).
- Break-glass / emergency access credentials: Rotate immediately after each use.

Automated rotation must be verified by confirming that the secrets management service successfully generates a new password, updates the database instance with the new credential, and updates the stored secret value.

### Manual Rotation

For database connections that do not support automatic rotation (e.g., legacy applications using environment files), a manual rotation process must be followed on the same rotation schedule. The manual procedure is:

1. Generate a new password that meets the Password Policy complexity requirements.
2. Update the database instance with the new credential.
3. Update the application's credential storage (environment file, configuration, or secrets manager).
4. Restart or reload the application to pick up the new credential.
5. Verify that the application successfully connects to the database with the new credential.
6. Document the rotation in the credential management log.

All manual password rotations must occur during a pre-defined maintenance window. For production services, the maintenance window is ____ (e.g., Saturdays 12:00 AM - 4:00 AM UTC). A calendar event or scheduled task must be created for each manual rotation with links to this process document.

## Rotation Procedure — Direct Secrets Retrieval (Pattern 1)

For services that retrieve credentials directly from the secrets management service:

1. The secrets management service automatically generates a new password according to the configured rotation schedule.
2. The service updates the database instance with the new credential.
3. The service updates the stored secret value with the new credential.
4. Applications retrieve the new secret value automatically at their next refresh interval or upon restart.
5. If applications do not support automatic secret refresh, a service restart or redeployment is required:
   - For container-based services, trigger a new deployment to pull the updated secret.
   - For virtual machine-based services, restart the application service.
6. Verify connectivity by checking application logs and database connection metrics.

## Rotation Procedure — Environment File (Pattern 2)

For services using environment variable files:

1. Retrieve the new password from the secrets management service console or via CLI.
2. Establish a secure session (SSH or equivalent) to the application server.
3. Navigate to the application directory containing the environment file.
4. Open the environment file for editing and locate the database password variable.
5. Replace the existing password value with the new password value.
6. Save the file and exit the editor.
7. Restart the application service using the appropriate service manager:
   - For systemd-managed services: `sudo systemctl restart [service-name]`
   - For container-based services: `docker compose down [service] && docker compose up -d [service]`
   - For other service managers, follow the documented restart procedure for that service.
8. Verify that the application starts successfully and connects to the database by checking logs and monitoring dashboards.

### Multi-Server Coordination

If multiple application servers use the same database credential and environment file pattern, the rotation must be coordinated:

1. Update the database credential first.
2. Update and restart each application server sequentially (not simultaneously) to avoid a complete service outage during the rotation.
3. Verify each server's connectivity before proceeding to the next.

## Post-Rotation Verification

After any password rotation (automated or manual), the following verification steps must be completed:

1. Confirm that all applications can connect to their respective databases (check application health endpoints and database connection pools).
2. Verify that no authentication errors appear in application or database logs for a monitoring period of ____ minutes after rotation.
3. Confirm that database monitoring dashboards show expected connection counts and query throughput.
4. Test a representative end-to-end transaction to confirm full functionality.
5. Document the rotation completion, including date, time, systems affected, and verification results, in the credential management log.

## Rotation Failure Response

If a password rotation fails — either the new credential is not applied correctly or applications cannot connect with the new credential:

1. Immediately revert to the previous credential if the old password is still valid and available.
2. If the old password has been invalidated, use the break-glass procedure to set a known password and restore connectivity.
3. Document the failure as an incident in the ticketing system.
4. Investigate the root cause before attempting another rotation.
5. Do not leave the database using a compromised or unknown password state.

## Access Control

Access to database credentials must be strictly controlled:

- Credentials stored in the secrets management service must be accessible only to authorized applications and personnel via IAM policies or equivalent access controls.
- Access to retrieve secret values manually must be logged and audited.
- Environment files containing database credentials must have file permissions restricted to the application user only (e.g., `600` or `400`).
- Shared or group credentials for database access are prohibited. Each application must have a unique database user and credential.
- Database credentials must never be committed to source code repositories, included in documentation, or transmitted over unencrypted channels.

## Roles and Responsibilities

| Role | Responsibility |
|------|----------------|
| ____ (e.g., Infrastructure Lead) | Owns the password rotation schedule; ensures secrets management platform availability |
| ____ (e.g., Database Administrators) | Execute database-side password updates; verify database connectivity post-rotation |
| Application Owners | Coordinate application-side credential updates; verify application connectivity post-rotation |
| ____ (e.g., Security Officer) | Reviews rotation compliance; audits credential access logs |
| ____ (e.g., IT Operations) | Supports rotation execution during maintenance windows; monitors for rotation failures |

## Compliance and Enforcement

Failure to rotate database credentials on schedule, failure to use the secrets management service for credential storage, or storage of database credentials in unauthorized locations (source code, shared documents, unencrypted files) is a violation of this process and the Password Policy and may result in disciplinary action as outlined in the Information Security Policy.

## Related Documents

- Password Policy
- Information Security Policy
- Encryption Policy
- System Access Control Policy
- Backup Policy

## Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | ____ | ____ | Initial version |
