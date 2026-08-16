# Yerigan Labs Security Baseline

## Status

Current baseline established following LAB-009.

This document describes validated controls and known limitations. It must not be interpreted as proof that the environment is Internet-inaccessible.

## Identity and Authentication

- Password manager established.
- Unique credentials established for current managed services.
- GitHub credential independently unique.
- SSH uses public-key authentication.
- SSH password authentication disabled.
- SSH root login disabled.
- Local Ubuntu password retained for sudo/local authentication.

## Secrets Management

- Runtime secrets excluded from Git.
- Pi-hole secret stored in ignored `.env`.
- Pi-hole `.env` permissions set to `600`.
- `.env.example` contains placeholder data only.
- Current repository reviewed for secret-like material.
- Repository history reviewed for historical secret exposure.
- No known active secret exposure identified during review.

## Host Firewall

UFW:

- Incoming: DENY
- Outgoing: ALLOW
- Routed: DENY

Explicit trusted-LAN access exists for required administrative and application services.

## Service Exposure

### SSH — TCP/22

Purpose: Server administration.

Controls:

- Trusted LAN firewall rule.
- Public-key authentication.
- Password authentication disabled.
- Root login disabled.

### Pi-hole — TCP/UDP 53

Purpose: LAN DNS.

Controls:

- Docker publishing bound to `192.168.1.203`.
- Trusted LAN firewall rules.

### Caddy — TCP/80

Purpose: Internal reverse proxy.

Controls:

- Bound to `192.168.1.203`.
- Trusted LAN firewall rule.

### Home Assistant — TCP/8123

Purpose: Home Assistant application access.

Controls:

- Trusted LAN firewall rule.
- Caddy Docker-network access.

Status:

Accepted temporary direct-LAN exposure.

Review after permanent network deployment.

### go2rtc — TCP/18554 and TCP/18555

Purpose: Home Assistant media/WebRTC functionality.

Observed:

- TCP/18554 bound to localhost.
- TCP/18555 listening broadly.

No explicit inbound UFW allow rule exists for TCP/18555.

Listener retained as a legitimate Home Assistant component.

### Portainer

No host port currently published.

### Uptime Kuma

No host port currently published.

### Homepage

No host port currently published.

## Known Deferred Verification

### Router

Not verified due to temporary GFiber management failure.

Review:

- Port forwards
- UPnP
- IPv6 firewall
- DMZ
- Remote administration

### External Reachability

Independent external port validation remains outstanding.

### Permanent Network

Security baseline must be reviewed after relocation and deployment of the permanent router/network.

### Home Assistant Direct Access

Review whether direct TCP/8123 LAN access remains necessary after the permanent reverse-proxy and Companion App architecture is established.

## Review Triggers

Review this baseline when:

- Network equipment changes.
- Moving to the permanent residence.
- A new externally reachable service is introduced.
- Firewall policy changes.
- Remote access is implemented.
- Cameras or additional IoT networks are deployed.
- A suspected credential or system compromise occurs.
