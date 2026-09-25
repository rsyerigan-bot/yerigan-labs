# GRC-001 — Interfaces and Data Flows

## Purpose

Document how information moves within the Yerigan Labs Cyber Range and how the system interacts with external supporting components.

This document focuses on:

- internal system communication
- administrative interfaces
- external dependencies
- intended data flows
- prohibited or unintended flows
- unresolved interface questions

## Internal Data Flows

### DF-001 — Kali 101 to Target 102

**Source:** Kali 101
**Destination:** Target 102
**Network:** `vmbr1`
**IPv4 Network:** `192.168.50.0/24`
**Direction:** Bidirectional
**Purpose:** Authorized cyber-range reconnaissance, enumeration, vulnerability validation, and controlled exploitation

This is the primary operational data flow within the cyber range.

Kali 101 and Target 102 are both attached to the isolated `vmbr1` virtual network.

Kali currently uses `192.168.50.10/24`.

Target 102 is attached to the same virtual network but currently has no IPv4 address assigned.

## Administrative Interfaces

### IF-001 — Administrative Workstation to Proxmox Management Interface

**Source:** Windows administrative workstation
**Destination:** Proxmox management interface
**Direction:** Inbound to external supporting infrastructure
**Boundary Status:** External administrative interface; both endpoints are outside the cyber-range boundary
**Purpose:** Create, configure, start, stop, inspect, and administer cyber-range resources

The administrative workstation and Proxmox management interface are outside the defined cyber-range boundary.

Administrative activity affects in-scope cyber-range resources through the external Proxmox management plane.

### IF-002 — Proxmox Host to Cyber-Range Resources

**Source:** Proxmox virtualization platform
**Destination:** Kali 101, Target 102, and `vmbr1`
**Direction:** Supporting infrastructure relationship
**Boundary Status:** External dependency to in-scope resources
**Purpose:** Provide compute, storage, and virtual-network resources required for the cyber range

The physical Proxmox host is treated as an external hosting dependency for GRC-001 rather than part of the cyber-range system boundary.

## Monitoring and Logging

### IF-003 — Cyber Range to Centralized Monitoring / Logging

**Source:** Cyber-range components
**Destination:** Centralized monitoring or logging services
**Direction:** Potential outbound telemetry (not yet validated)
**Boundary Status:** Potential boundary-crossing interface
**Purpose:** Security monitoring, troubleshooting, evidence collection, and later defensive-security exercises

The exact telemetry sources and destinations have not yet been validated for the cyber-range VMs.

This interface remains an open item until live configuration confirms whether Kali 101 or Target 102 currently sends logs or metrics to centralized monitoring services.

## Prohibited or Unintended Flows

### PF-001 — Target 102 to Home LAN

**Expected State:** No direct connectivity

Target 102 should not have direct connectivity to the normal home LAN or other household systems.

The intentionally vulnerable target must remain isolated from production and household networks.

### PF-002 — Cyber-Range Bridge to Home LAN

**Expected State:** No routed or bridged connectivity

`vmbr1` should not provide a routed or bridged path to the normal home LAN.

Host-side validation confirmed that `vmbr1` has no IPv4 address, no physical bridge port (`bridge-ports none`), and no route in the Proxmox routing table. These observations support its use as an isolated Layer-2 cyber-range network.

### PF-003 — Kali 101 Acting as an Unintended Router

**Expected State:** No forwarding between the cyber-range network and external networks

Kali 101 currently has one non-loopback network interface.

If a second interface is added in the future for software updates or external connectivity, IP forwarding and routing behavior must be reviewed before use.

## Trust-Boundary Crossings

The following relationships cross the defined cyber-range boundary:

1. Proxmox hosting and management infrastructure to in-scope cyber-range resources
2. potential cyber-range telemetry to external monitoring/logging services
3. any future externally connected Kali interface to the isolated cyber-range environment

The administrative workstation-to-Proxmox management connection occurs entirely outside the cyber-range boundary, although it forms part of the overall administrative path used to manage in-scope resources.

The following communication remains inside the boundary:

1. Kali 101 to Target 102 over `vmbr1`

## Open Items

- Assign or restore an appropriate IPv4 address to Target 102 before the next active IPv4-based cyber-range exercise.
- Validate whether cyber-range VMs currently send logs or metrics to centralized monitoring services.
- Reassess network isolation if Kali 101 receives a second network interface in the future.
- Confirm forwarding remains disabled if any multi-homed cyber-range system is introduced.

## Current Assessment

The cyber range currently has a narrow and understandable communication model.

Its intended operational traffic remains on the isolated `vmbr1` network, while management occurs through external supporting infrastructure.

The primary unresolved interface question is whether centralized monitoring or logging currently receives telemetry from the cyber-range systems.
