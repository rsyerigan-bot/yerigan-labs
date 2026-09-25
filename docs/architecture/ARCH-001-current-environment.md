# ARCH-001 — Current Homelab Architecture

**Status:** Active
**Last Updated:** September 25, 2026

## Purpose

This document records the current Yerigan Labs environment so future changes can be compared against a known baseline.

The architecture supports three goals:

1. Home and family services
2. Career development in infrastructure and cybersecurity
3. A future MSP, MSSP, or consulting capability

## Physical Infrastructure

### Proxmox Host

- Device: HP ProDesk 600 G4 SFF
- Processor: Intel Core i7-7700
- Memory: 16 GB RAM
- Storage: Samsung 870 EVO 500 GB SSD
- Hypervisor: Proxmox VE
- Management address: `192.168.4.10`
- Filesystem: ext4

### Administrative Workstation

- Operating system: Windows
- Primary terminal: Windows Terminal
- Administration tools:
  - OpenSSH
  - Visual Studio Code
  - VS Code Remote SSH
  - Web browser

### Additional Equipment

Available for future use:

- Dell OptiPlex 3010 SFF
- Additional laptops and desktop hardware
- NAS hardware

## Network

```text
Internet
   |
   v
eero Gateway
192.168.4.1
   |
   v
Home LAN: 192.168.4.0/22
   |
   +-- Proxmox Host
   |      192.168.4.10
   |
   +-- Ubuntu Docker Host
          192.168.4.203
          ens18
          Static IPv4
```

The current home LAN is `192.168.4.0/22`.

The environment currently uses a flat trusted home LAN. VLAN and IoT segmentation remain planned future improvements.

### DNS

Pi-hole provides LAN DNS at:

    192.168.4.203

The Ubuntu Docker host uses Pi-hole as its system DNS resolver.

The eero gateway distributes the Pi-hole address to LAN clients through DHCP.

Current internal service records use the `yerigan.home.arpa` namespace and resolve to `192.168.4.203`.

## Virtualization Architecture

```text
HP ProDesk
└── Proxmox VE
    ├── Ubuntu Server VM
    │   └── Docker Engine
    ├── KALI01
    └── TARGET01
```

The Kali and target systems support the isolated cybersecurity range and are not part of the normal home-service path.

## Ubuntu Server

- Hostname: `ubuntu`
- LAN address: `192.168.4.203/22`
- Default gateway: `192.168.4.1`
- Interface: `ens18`
- Primary role: Docker application host
- QEMU Guest Agent: Enabled
- SSH administration: Enabled
- Password-based SSH login: Disabled
- Direct root SSH login: Disabled
- Authentication: Passphrase-protected Ed25519 key
- Host firewall: UFW

## Docker Architecture

The Ubuntu host runs the current application, monitoring, and logging workloads.

Current containers include:

- Caddy
- Pi-hole
- Homepage
- Portainer
- Uptime Kuma
- Home Assistant
- Prometheus
- Grafana
- Node Exporter
- cAdvisor
- Loki
- Grafana Alloy

### Shared Proxy Network

Caddy and supported backend applications use the external Docker network:

    proxy

Current subnet:

    172.22.0.0/16

This allows supported web applications to remain unpublished on the host while Caddy provides the LAN-facing HTTP entry point.

## Service Access

### Caddy

Purpose:

- Internal reverse proxy
- Friendly-name access to web applications
- Internal Home Assistant webhook forwarding

Host publication:

    192.168.4.203:80

### Pi-hole

Purpose:

- LAN DNS
- DNS filtering
- Internal service-name resolution

Host publication:

    192.168.4.203:53 TCP/UDP

Administrative access:

    http://dns.yerigan.home.arpa

### Homepage

Purpose:

- Central operations dashboard
- Service launcher
- Infrastructure overview

Access:

    http://home.yerigan.home.arpa

Repository configuration:

    infrastructure/docker/homepage/

Homepage has no direct host web-port publication.

### Portainer

Purpose:

- Docker administration
- Container lifecycle management
- Log and resource inspection

Access:

    http://portainer.yerigan.home.arpa

Portainer has no direct host web-port publication.

### Uptime Kuma

Purpose:

- Availability monitoring
- HTTP and ping health checks
- Service status visibility

Access:

    http://status.yerigan.home.arpa

Uptime Kuma has no direct host web-port publication.

### Home Assistant

Purpose:

- Home automation
- Device integration
- Family-facing automation capability

Access:

    http://ha.yerigan.home.arpa

Home Assistant currently listens on host TCP/8123.

Direct trusted-LAN TCP/8123 access remains temporarily permitted while the long-term access architecture is reviewed.

### Grafana

Purpose:

- Infrastructure metrics visualization
- Monitoring dashboards
- Logging and observability workflows

Access:

    http://grafana.yerigan.home.arpa

Grafana has no direct host web-port publication.

### Monitoring

Prometheus collects infrastructure metrics.

Node Exporter provides host metrics.

cAdvisor provides container metrics.

These services support the monitoring stack without requiring direct LAN-facing web publication in the current design.

### Logging

Loki provides centralized log storage.

Grafana Alloy collects and forwards logging data into the logging stack.

## Administration Paths

### Primary Administration

```text
Windows Workstation
        |
        | SSH using ubuntu-lab profile
        | Ed25519 private key
        v
Ubuntu Server
```

Current Ubuntu destination:

    192.168.4.203

### Emergency Administration

```text
Windows Browser
        |
        v
Proxmox Web Interface
        |
        v
Ubuntu VM Console
```

Proxmox management:

    https://192.168.4.10:8006

The Proxmox console remains the fallback administration path if Ubuntu network or SSH access fails.

## Host Firewall Baseline

UFW is active.

Default policy:

- Incoming: deny
- Outgoing: allow
- Routed: deny

Current explicit rules include:

- TCP/22 from `192.168.4.0/22` for SSH
- TCP/53 from `192.168.4.0/22` for Pi-hole DNS
- UDP/53 from `192.168.4.0/22` for Pi-hole DNS
- TCP/80 from `192.168.4.0/22` for Caddy
- TCP/8123 from `192.168.4.0/22` for temporary Home Assistant direct access
- TCP/8123 from `172.22.0.0/16` for Caddy-to-Home-Assistant communication

Docker publication and host listeners must be evaluated separately from UFW because Docker networking can affect effective exposure.

## Current Host Listener Notes

Validated LAN-facing or broadly bound listeners include:

- SSH TCP/22
- Pi-hole TCP/UDP 53 bound to `192.168.4.203`
- Caddy TCP/80 bound to `192.168.4.203`
- Home Assistant TCP/8123
- Home Assistant discovery-related UDP listeners
- go2rtc TCP/18554 bound to localhost
- go2rtc TCP/18555 broadly bound

Broad listeners do not by themselves establish Internet reachability. Router, IPv6, firewall, and external-reachability controls must be evaluated separately.

## Configuration Management

The Yerigan Labs Git repository is the intended source of truth for infrastructure configuration and documentation.

Current repository-managed service configuration includes:

- Caddy
- Homepage
- Pi-hole
- Portainer
- Monitoring stack
- Logging stack

Home Assistant retains application configuration outside the repository and therefore remains a configuration-drift consideration.

Historical runtime directories may remain temporarily during migrations for rollback purposes but are not authoritative after ownership transfer has been validated.

## Security Controls Currently Implemented

- Static IPv4 addressing for core infrastructure hosts
- SSH public-key authentication
- Passphrase-protected administrative SSH key
- SSH password authentication disabled
- Direct root SSH login disabled
- UFW deny-by-default inbound policy
- LAN-scoped administrative and service firewall rules
- Pi-hole LAN DNS
- Caddy internal reverse proxy
- Minimized Docker host-port publication
- Git-based infrastructure configuration
- Prometheus/Grafana monitoring
- Loki/Alloy centralized logging
- Isolated cybersecurity range
- Proxmox console fallback for recovery

## Current Limitations and Deferred Work

- Flat home LAN without VLAN segmentation
- IoT and camera segmentation not yet implemented
- Home Assistant direct TCP/8123 LAN access remains under review
- go2rtc TCP/18555 requires continued exposure awareness
- Router port-forward configuration requires formal validation
- UPnP requires formal review
- IPv6 firewall behavior requires formal review
- Independent external-reachability testing remains outstanding
- Backup and disaster-recovery capability is not yet mature
- Home Assistant configuration is not fully repository-managed
- Trusted internal HTTPS has not yet been implemented

## Planned Architecture Improvements

### Identity and Administrative Access

Continue improving administrative identity, authorization, and access-management controls.

### Backup and Recovery

Implement tested configuration and service backup/recovery procedures.

### Network Security

Planned improvements include:

- VLAN segmentation
- Separate infrastructure, family, IoT, guest, and camera networks
- Inter-VLAN firewall policy
- Least-privilege communication between services
- Secure remote access
- Formal IPv6 and perimeter-security validation

### Home and Family

Continue adding services that provide practical household value while maintaining operational and security discipline.

### Cybersecurity Range

Continue expanding isolated offensive and defensive security exercises without exposing intentionally vulnerable systems to the trusted home LAN or Internet.

## Design Notes

The architecture prioritizes incremental engineering over unnecessary complexity.

Changes should be designed, implemented, validated, documented, and version-controlled.

Security conclusions must distinguish between:

- host listener state
- Docker publication
- host firewall policy
- LAN reachability
- router/perimeter controls
- IPv4 and IPv6 behavior
- independently validated external reachability

A control should not be reported as validated until evidence supports that conclusion.

Future changes should continue supporting all three Yerigan Labs pillars:

1. Practical benefit to the household
2. Career development
3. Reusable capability for a future business
