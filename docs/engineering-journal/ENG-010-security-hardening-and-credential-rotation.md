# ENG-010 — Security Hardening and Credential Rotation

## Context

During development of LAB-009, concern was raised regarding credential reuse and the security implications of maintaining a public Yerigan Labs GitHub repository.

Rather than continuing platform expansion, engineering work was paused to assess and reduce the current security risk.

The objective was not simply to change passwords, but to evaluate credential security, secrets management, repository exposure, host attack surface, and recovery procedures.

## Initial Risks

The assessment identified several areas requiring review:

- A common password had been reused across multiple lab services.
- The Yerigan Labs repository is public.
- The external exposure of the Ubuntu/Docker host had not been fully characterized.
- Docker-published ports required review.
- Router configuration could not currently be inspected due to a temporary GFiber service/account condition.
- Home Assistant and other services required credential separation.

## Credential Management

Bitwarden was established as the password manager.

The following principles were adopted:

- Unique credentials for each service.
- Generated passwords stored in the password manager.
- Credentials must not be placed in Git.
- Credentials must not be placed in documentation.
- Credentials should not be passed directly in shell commands when avoidable.
- Credential changes must be validated before terminating known-good sessions.
- Recovery capability must be considered part of authentication design.

## Credential Rotation

Credentials were reviewed or rotated for:

- Pi-hole
- Home Assistant
- Portainer
- Uptime Kuma
- Ubuntu

The GitHub credential was already unique and was not rotated without cause.

Router and Wi-Fi credential review was deferred because the GFiber management interface was unavailable during temporary provider repair work.

## Pi-hole Secrets Management

Pi-hole uses an environment variable reference in Docker Compose:

`PIHOLE_WEBPASSWORD`

The actual value is stored in a local `.env` file.

Validation confirmed:

- `.env` is ignored by Git.
- `.env` was never tracked in repository history.
- `.env.example` contains only a placeholder.
- The committed Compose configuration contains only the variable reference.
- The `.env` file permissions were reduced to owner read/write only (`600`).

Pi-hole authentication and DNS resolution were tested after credential rotation.

## Home Assistant Recovery Event

During Home Assistant password rotation, the newly configured password did not initially permit authentication.

The existing authenticated session was deliberately preserved.

The actual Home Assistant authentication username was identified as separate from the display name and email address.

Password recovery was performed using the Home Assistant container authentication CLI.

The service required a restart before the recovered credential authenticated successfully.

This incident demonstrated the importance of:

- Preserving known-good administrative access during credential changes.
- Understanding the difference between display identity and authentication identity.
- Testing credentials using a fresh session.
- Maintaining documented recovery procedures.

## Ubuntu Authentication Review

Ubuntu SSH configuration was reviewed.

Validated controls included:

- Root SSH login disabled.
- SSH password authentication disabled.
- Public-key authentication enabled.
- One authorized SSH key present.
- `randy` is the only normal human user identified.
- `sudo` requires the local Ubuntu account credential.

The Ubuntu password was rotated.

Validation included:

- Successful `sudo` authentication using the new password.
- Successful authentication from a new SSH session.
- Successful `sudo` authentication from the new SSH session.

SSH keys were not rotated because no evidence of compromise was identified.

## Repository Secret Audit

The public Git repository was reviewed for accidental secret exposure.

Current tracked content was searched for:

- Environment files
- Private keys
- Passwords
- Secrets
- API keys
- Access tokens
- Authentication tokens

Repository history was also reviewed for historical secret-like files and content.

Findings:

- No real `.env` file was tracked.
- No SSH private key material was identified.
- No obvious active credentials were identified.
- `.env.example` contains only placeholder data.
- Documentation references to passwords and secrets were non-sensitive.
- The Pi-hole Compose history contained only the environment-variable reference.

No repository history rewrite was required.

## Host Attack Surface Review

Listening services and Docker-published ports were inventoried.

The host firewall was confirmed active with:

- Default incoming: deny
- Default outgoing: allow
- Default routed: deny

SSH is permitted from the trusted LAN only.

Pi-hole DNS is permitted from the trusted LAN only.

Caddy HTTP is permitted from the trusted LAN only.

Home Assistant TCP/8123 is permitted from the trusted LAN and the Caddy Docker network.

## Pi-hole Network Hardening

Pi-hole initially published DNS on all host interfaces.

Docker Compose was changed to bind DNS specifically to:

`192.168.1.203:53`

for TCP and UDP.

Post-change validation confirmed:

- Docker Compose configuration valid.
- Pi-hole container recreated successfully.
- TCP/53 bound to `192.168.1.203`.
- UDP/53 bound to `192.168.1.203`.
- DNS resolution remained functional.

This reduces unnecessary network exposure.

Docker documentation confirms that published ports without a specified host address bind to all host addresses by default.

## go2rtc Investigation

TCP/18555 was identified as an unexplained listener during the attack-surface review.

Process inspection determined that it belonged to Home Assistant's managed go2rtc service.

Observed listeners:

- `127.0.0.1:18554`
- `*:18555`

The listener was retained because it is a legitimate Home Assistant component used for media/WebRTC functionality.

No explicit UFW allow rule exists for TCP/18555.

## Accepted and Deferred Risks

### Home Assistant TCP/8123

Direct Home Assistant access remains available from the trusted LAN.

This is currently accepted because the Companion App and local integrations are under active development.

The requirement should be reviewed after deployment of the permanent network.

### Router Perimeter

GFiber router configuration could not be reviewed because temporary provider repair work caused the management application to incorrectly report service as unavailable.

The following remain unverified:

- Port forwarding
- UPnP mappings
- IPv6 perimeter firewall
- Remote administration
- DMZ configuration

This review must be completed when router management becomes available or after migration to the permanent network.

### External Exposure

Host-side controls were reviewed, but external reachability was not independently validated from outside the network.

This remains deferred.

## Lessons Learned

- Credential reuse creates unnecessary blast radius.
- Password managers make unique credentials operationally practical.
- Authentication recovery must be designed before it is needed.
- Listening does not mean firewall-allowed.
- Firewall-allowed does not necessarily mean Internet-accessible.
- Docker EXPOSE and Docker port publishing are different concepts.
- Published ports should be intentionally bound to required interfaces.
- A public repository requires both current-state and historical secret review.
- Deleting a secret from the current repository does not remove it from Git history.
- A credential should be rotated when compromise is suspected; unrelated credentials should not be rotated without reason.
- Security controls should be validated rather than assumed.

## Result

The known shared-password risk across the current Yerigan Labs environment was substantially reduced.

Credential isolation, secrets handling, repository hygiene, host firewall posture, SSH authentication, and Docker network exposure were reviewed and improved.

Remaining perimeter and external-exposure verification is explicitly documented rather than assumed complete.
