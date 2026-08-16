# SEC-001 — Credential and Secrets Management Standard

## Purpose

Establish minimum credential and secrets-management requirements for Yerigan Labs.

## Requirements

### Unique Credentials

Every interactive service must use a unique credential unless a documented technical requirement prevents it.

Credential reuse between services is prohibited.

### Password Manager

Human-managed credentials must be stored in the approved password manager.

Passwords must not be stored in:

- Git repositories
- Documentation
- Tickets
- Source code
- Shell scripts
- Unencrypted notes

### Password Generation

Service passwords should be randomly generated where supported.

Length and complexity should meet or exceed application requirements.

### Secrets in Infrastructure as Code

Secrets must not be hard-coded into committed infrastructure configuration.

Use:

- Environment variables
- Local ignored `.env` files
- Secret-management mechanisms

Committed `.env.example` files must contain placeholders only.

### File Permissions

Local files containing secrets must use the minimum permissions necessary.

Owner-only read/write (`600`) is the default expectation for simple local secret files unless another permission model is technically required.

### Credential Rotation

Credentials must be rotated when:

- Reuse is discovered.
- Exposure is suspected or confirmed.
- A credential is accidentally committed.
- A relevant account or system is compromised.
- Operational or organizational requirements mandate rotation.

Credentials should not be rotated without reason when doing so introduces unnecessary operational risk.

### Validation

Credential changes are not complete until authentication is tested using a fresh session.

Existing known-good administrative sessions should remain open until the replacement credential is validated.

### Recovery

Authentication systems must have a documented recovery path where practical.

Recovery capability is part of security design.

### Repository Security

Public repositories must not contain active credentials.

Secret review should include both:

- Current tracked files.
- Git history.

Removal from the current working tree does not by itself remove a secret from repository history.

### SSH

Remote administrative SSH should prefer public-key authentication.

Password-based SSH and remote root login should remain disabled unless a documented requirement exists.

### Network Services

Network listeners must have a documented purpose.

Docker ports must only be published when required.

Published services should bind to the narrowest practical host interface.

Firewall controls must use least-required access.

## Exceptions

Exceptions require documentation describing:

- Requirement
- Risk
- Compensating control
- Planned remediation or review point
