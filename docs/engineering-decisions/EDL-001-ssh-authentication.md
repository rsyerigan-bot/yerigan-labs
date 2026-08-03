# EDL-001 — SSH Authentication Method

**Status:** Accepted  
**Date:** August 3, 2026

## Context

The Ubuntu Docker host requires secure remote administration from a Windows workstation. Password-only authentication permits repeated password attempts and depends on a reusable account credential.

## Decision

Use passphrase-protected Ed25519 SSH keys for remote administration.

Disable:

- Password authentication
- Keyboard-interactive authentication
- Direct root login

Retain:

- Public-key authentication
- Local account password for `sudo`
- Proxmox console as an emergency recovery path

## Alternatives Considered

### Password-only authentication

Advantages:

- Minimal setup
- Familiar user experience

Disadvantages:

- Susceptible to password guessing
- Reusable credential
- Less suitable for automation
- Common target for brute-force attacks

### RSA SSH keys

Advantages:

- Broad compatibility
- Mature and widely understood

Disadvantages:

- Larger keys
- Ed25519 is simpler and preferable for this modern environment

### SSH certificate authority

Advantages:

- Centralized trust
- Better lifecycle management at scale
- Appropriate for larger organizations

Disadvantages:

- Unnecessary complexity for the current single-administrator lab
- Additional infrastructure and operational overhead

## Rationale

Ed25519 provides strong modern cryptography, compact keys, fast authentication, and broad support across Windows and Ubuntu.

A private-key passphrase reduces the impact of private-key theft. Disabling password-style SSH methods reduces the remotely exposed authentication surface.

## Trade-offs

- Loss of the private key could prevent remote access.
- The passphrase must be securely remembered or stored.
- Additional client devices require their own authorized keys.
- Recovery procedures must remain documented and tested.

## Consequences

Positive:

- Stronger remote authentication
- Reduced password-guessing exposure
- Easier future automation
- Alignment with common enterprise Linux practices

Negative:

- More credential lifecycle management
- Key backup and revocation procedures are required
- Additional setup for each administrative workstation

## Future Improvements

- Use `ssh-agent` for secure passphrase caching.
- Add separate keys for separate administrative devices.
- Document key revocation.
- Consider hardware-backed keys.
- Consider an SSH certificate authority if the environment grows substantially.