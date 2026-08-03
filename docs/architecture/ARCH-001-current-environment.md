# ARCH-001 — Current Homelab Architecture

**Status:** Active  
**Last Updated:** August 3, 2026

## Purpose

This document records the current Yerigan Labs environment so future changes can be compared against a known baseline.

The architecture currently supports three goals:

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
- Management address: `192.168.1.10`
- Filesystem: ext4

### Administrative Workstation

- Operating system: Windows
- Administration tools:
  - Windows PowerShell
  - Visual Studio Code
  - VS Code Remote SSH
  - Web browser

### Additional Equipment

Available for future use:

- Dell OptiPlex 3010 SFF
- Two laptops
- Older desktop PC
- NAS hardware

## Network

```text
Internet
    |
GFiber Router / Google Mesh
Gateway: 192.168.1.1
    |
Home LAN: 192.168.1.0/24
    |
    +-- Proxmox Host
    |      IP: 192.168.1.10
    |
    +-- Ubuntu Server VM
           IP: 192.168.1.203
           Interface: ens18
           Addressing: Static IPv4
           The Proxmox host is connected through a Google Mesh node using Ethernet.

The environment currently uses a flat home network. VLANs and security segmentation are planned but have not yet been implemented.

Virtualization Architecture
HP ProDesk
└── Proxmox VE
    └── Ubuntu Server VM
        ├── Docker Engine
        ├── Portainer
        ├── Pi-hole
        ├── Homepage
        └── Uptime Kuma
Ubuntu Server VM
Hostname: ubuntu
IP address: 192.168.1.203/24
Default gateway: 192.168.1.1
Primary role: Docker application host
QEMU Guest Agent: Enabled
SSH administration: Enabled
Password-based SSH login: Disabled
Direct root SSH login: Disabled
Authentication: Passphrase-protected Ed25519 key
Containerized Services
Portainer

Purpose:

Docker administration
Container lifecycle management
Stack deployment
Log and resource inspection

Access:

https://192.168.1.203:9443
Pi-hole

Purpose:

DNS filtering
Ad and tracker blocking
DNS learning and experimentation

Access:

http://192.168.1.203:8080/admin

Upstream resolver:

Quad9 filtered DNS with DNSSEC

Pi-hole has been tested successfully, but router-wide DNS use should remain controlled until backup and recovery procedures are mature.

Homepage

Purpose:

Central operations dashboard
Service launcher
Current and planned infrastructure overview

Access:

http://192.168.1.203:3000

Configuration location:

/home/randy/docker/homepage/config
Uptime Kuma

Purpose:

Availability monitoring
HTTP and ping health checks
Service status visibility

Access:

http://192.168.1.203:3001

Current monitors:

Proxmox
Ubuntu Server
Portainer
Pi-hole
Homepage

Current status:

All configured monitors healthy
Administration Path
Windows Workstation
    |
    | SSH using ubuntu-lab profile
    | Ed25519 private key
    v
Ubuntu Server

Windows SSH profile:

Host ubuntu-lab
    HostName 192.168.1.203
    User randy
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

Emergency administration path:

Windows Browser
    |
Proxmox Web Interface
    |
Ubuntu VM Console
Security Controls Currently Implemented
Static IPv4 addressing for infrastructure hosts
SSH public-key authentication
Passphrase-protected private key
SSH password authentication disabled
SSH keyboard-interactive authentication disabled
Direct root SSH login disabled
QEMU Guest Agent enabled
Proxmox snapshot created after stable core deployment
Pi-hole upstream DNS configured to Quad9
No intentional internet-facing port forwarding
Current Limitations
Flat network with no VLAN segmentation
No host firewall policy yet
No reverse proxy
No trusted internal HTTPS certificates
No NAS-based backup target
No Git remote
No centralized logging or SIEM
No automatic security update policy documented
Docker services currently share one Ubuntu host
Several services use raw IP addresses and ports
Planned Architecture Improvements
Near Term
Git version control for configuration and documentation
UFW firewall baseline
Automatic security updates
Configuration backup process
NAS integration for backups
Monitoring metrics with Prometheus and Grafana
Enterprise Lab
Windows Server
Active Directory Domain Services
DNS
Group Policy
Windows 11 domain client
Wazuh or another SIEM
Vulnerability scanning
Home and Family
NAS storage for photos, music, video, and documents
Jellyfin media streaming
Immich or another family photo platform
Home Assistant
Secure IoT integration
Family-facing dashboard
Network Security
VLAN segmentation
Separate infrastructure, family, IoT, guest, and camera networks
Inter-VLAN firewall rules
Least-privilege communication between services
Secure remote access through VPN
Reverse proxy and certificate management
Design Notes

The current architecture intentionally prioritizes simplicity while foundational skills and documentation practices are established.

Security controls will be added incrementally before exposing services beyond the trusted home network.

The future design should preserve value across all three Yerigan Labs pillars:

Practical benefit to the household
Career development
Reusable capability for a future business