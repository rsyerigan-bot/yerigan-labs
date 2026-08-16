# RB-009 — Home Assistant Credential Recovery

## Purpose

Recover access to the Home Assistant administrator account when the normal password is unknown or a password change fails.

## Scope

Home Assistant Container deployment used by Yerigan Labs.

## Safety Requirements

Before beginning:

1. Preserve any currently authenticated Home Assistant browser session.
2. Preserve existing SSH access to the Ubuntu host.
3. Do not delete Home Assistant `.storage` authentication files.
4. Do not rebuild the Home Assistant container as a first recovery action.
5. Never record the new password in this runbook, Git, tickets, or shell history.

## Procedure

### 1. Access the Home Assistant container

```bash
docker exec -it homeassistant bash
