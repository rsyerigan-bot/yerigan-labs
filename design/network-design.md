# Yerigan Labs Network Design

**Status:** Draft
**Scope:** Current flat network and future segmented architecture
**Last Updated:** September 25, 2026

## Purpose

This document defines the intended network structure for Yerigan Labs.

The design must support:

1. Reliable family services
2. Safe enterprise and cybersecurity labs
3. Secure IoT and camera integration
4. Clear administrative boundaries
5. Future MSP or MSSP simulation and reusable client patterns

## Current Network

### Home LAN

```text
Network: 192.168.4.0/22
Gateway: 192.168.4.1
Proxmox: 192.168.4.10
Ubuntu Docker Host: 192.168.4.203
LAN DNS: 192.168.4.203
```

The current home network uses an eero gateway and remains a flat trusted LAN.

Pi-hole provides LAN DNS. The eero gateway distributes the Pi-hole address to LAN clients through DHCP.

Internal services use the `yerigan.home.arpa` namespace.

### Current Service Path

```text
LAN Client
    |
    | DNS
    v
Pi-hole
192.168.4.203
    |
    | friendly service name resolves to 192.168.4.203
    v
Caddy
192.168.4.203:80
    |
    v
Docker Services
```

Where practical, web applications are accessed through Caddy rather than directly published application ports.

### Cybersecurity Range

The intentionally vulnerable cybersecurity range remains isolated from the normal home-service path.

Kali and target systems must not be intentionally exposed to the trusted home LAN or Internet merely for convenience.

## Current Design Limitations

The current flat LAN does not provide strong network-level separation between infrastructure, family devices, IoT devices, cameras, and other trusted clients.

Current limitations include:

- No production VLAN segmentation
- No dedicated infrastructure VLAN
- No dedicated IoT VLAN
- No dedicated camera VLAN
- No dedicated guest security policy designed by Yerigan Labs
- Inter-segment firewall policy has not yet been designed
- IPv6 perimeter behavior requires formal validation
- UPnP requires formal review
- Independent external-reachability validation remains outstanding

These limitations are documented design debt rather than evidence that the current network is Internet-exposed.

## Future Segmented Architecture

The future architecture should introduce logical security boundaries without adding complexity that does not provide operational or security value.

Expected security zones include:

- Infrastructure
- Family / trusted clients
- IoT
- Cameras
- Guest
- Cybersecurity lab

Exact VLAN IDs, IPv4 subnets, IPv6 behavior, routing policy, and firewall rules are intentionally not defined in this draft.

Those decisions should be made when the network hardware and operational requirements are ready for implementation.

## Segmentation Requirements

Future segmentation should follow these principles:

- Default-deny communication between security zones where practical
- Explicitly permit required cross-zone services
- Preserve family usability
- Keep infrastructure administration restricted to trusted administrative paths
- Allow IoT devices only the access required for their function
- Restrict camera access according to operational need
- Keep intentionally vulnerable lab systems isolated
- Preserve Pi-hole DNS where appropriate
- Preserve controlled access to shared home services
- Document exceptions rather than relying on undocumented permissive rules

## Administrative Requirements

Network infrastructure should support:

- Documented management addresses
- Configuration backup and recovery
- Clear device and port naming
- Change validation
- Firewall-rule documentation
- Monitoring where practical
- Recovery access if normal network administration fails

## Design Status

The current flat network is operational.

The segmented architecture remains a future design and implementation effort.

This document should be updated when VLAN-capable routing and switching architecture, addressing, and firewall policy are formally selected.
