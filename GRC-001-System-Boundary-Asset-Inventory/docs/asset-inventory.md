# GRC-001 — Asset Inventory

## Purpose

Document the assets that make up the Yerigan Labs Cyber Range and identify their role, ownership, boundary status, and relevant configuration details.

This inventory is intentionally limited to the assets required to operate the defined cyber-range system.

## Inventory

| Asset ID | Asset Name | Asset Type | Role | Platform / OS | Host / Parent | Network | IP Address | Boundary Status | Owner / Admin | Operational Importance | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|
| CR-VM-001 | Kali 101 | Virtual Machine | Attacker / assessment workstation | Kali Linux | Proxmox host | vmbr1 | 192.168.50.10 | In scope | Randall Yerigan | Required | Authorized system used for reconnaissance, testing, and controlled exploitation |
| CR-VM-002 | Target 01 / VM 102 | Virtual Machine | Intentionally vulnerable target | Metasploitable 2 | Proxmox host | vmbr1 | None currently assigned | In scope | Randall Yerigan | Required | Training target used only inside the isolated cyber range |
| CR-NET-001 | vmbr1 Cyber Range Bridge | Virtual Network | Provides isolated connectivity between attacker and target | Proxmox Linux Bridge | Proxmox host | 192.168.50.0/24 | N/A | In scope | Randall Yerigan | Required | Must remain isolated from the home LAN and other production/home systems |

## Optional Configuration Assets

The following virtual components may also be tracked if more detailed configuration management is needed:

| Asset ID | Asset Name | Asset Type | Parent Asset | Purpose | Boundary Status | Notes |
|---|---|---|---|---|---|---|
| CR-NIC-001 | Kali Range NIC | Virtual NIC | CR-VM-001 | Connect Kali to the isolated cyber-range network | In scope | Should not provide unintended routing to external networks |
| CR-NIC-002 | Target Range NIC | Virtual NIC | CR-VM-002 | Connect target to the isolated cyber-range network | In scope | Should remain isolated from the normal home LAN |
| CR-DISK-001 | Kali Virtual Disk | Virtual Disk | CR-VM-001 | Operating system and assessment-tool storage | In scope | Track only if configuration or recovery requirements justify it |
| CR-DISK-002 | Target Virtual Disk | Virtual Disk | CR-VM-002 | Training target operating system and vulnerable services | In scope | Snapshot or restore state may become important in future labs |

## External Supporting Assets

These assets support the cyber range but are outside the defined system boundary.

| Asset ID | Asset Name | Asset Type | Role | Boundary Status | Relationship to System |
|---|---|---|---|---|---|
| EXT-HOST-001 | Proxmox Host | Physical Host | Provides virtualization platform | Out of scope | Hosts the in-scope VMs and virtual bridge |
| EXT-MGMT-001 | Proxmox Management Interface | Management Interface | Administrative control plane | Out of scope | Used to configure and manage the cyber range |
| EXT-ENDPOINT-001 | Windows Administrative Workstation | Physical Endpoint | Administrator workstation | Out of scope | Used to access Proxmox and manage the environment |
| EXT-MON-001 | Centralized Monitoring / Logging | Supporting Service | Observability and evidence collection | Out of scope | May receive logs or monitoring data from the cyber range |

## Inventory Rules

An asset is included in the in-scope inventory when it:

1. is required for the cyber range to perform its intended function,
2. operates within the defined system boundary, or
3. directly participates in authorized cyber-range activity.

Supporting systems are documented separately when they provide hosting, administration, monitoring, or other services without being part of the defined operational boundary.

## Open Items

- Determine whether Target 102 should be assigned a static IPv4 address before the next active cyber-range exercise.
- Confirm whether centralized monitoring/logging currently receives telemetry from either cyber-range VM.
- Decide whether VM snapshots should be tracked as configuration or recovery assets in later labs.

## Validation Notes

- Kali 101 was directly verified at `192.168.50.10/24`.
- Kali 101 currently has one non-loopback network interface.
- Target 102 currently has one network interface attached to `vmbr1`.
- Target 102 currently has no IPv4 address assigned.
- Target 102 does have an IPv6 link-local address, which does not provide normal IPv4 connectivity within the intended `192.168.50.0/24` range.
- `vmbr1` was directly verified as operational with no host-side IPv4 address.
- The cyber-range IPv4 network is `192.168.50.0/24`.

## Status

Initial system asset inventory validated against the live environment.

Core in-scope assets, network attachment, boundary status, and current addressing state have been documented. Remaining open items concern future configuration and monitoring decisions rather than unidentified assets.
