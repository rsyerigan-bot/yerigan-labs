# GRC-001 — System Boundary

## System Name

Yerigan Labs Cyber Range

## System Purpose

The Yerigan Labs Cyber Range provides an isolated and authorized environment for cybersecurity training, network reconnaissance, vulnerability validation, and controlled exploitation.

## Boundary Decision

The system boundary includes only the components required to conduct cyber-range activity within an isolated training network.

### In-Scope Components

- Kali Linux VM 101
- Target VM 102
- isolated Proxmox virtual bridge connecting the attacker and target systems

These components collectively form the operational cyber range.

## Out-of-Scope Supporting Components

The following components support administration or hosting but are not considered part of the cyber-range authorization boundary:

- physical Proxmox host
- Proxmox management interface
- Windows administrative workstation

The Proxmox host provides the virtualization platform on which the in-scope virtual machines and network bridge operate, but it is treated as an external hosting dependency for this exercise.

The Proxmox management interface is used to administer the environment but is not part of the cyber-range operational boundary.

The Windows administrative workstation is treated as an external administrator endpoint.

## External Monitoring and Logging

Centralized monitoring and logging services may observe cyber-range activity but are not currently considered part of the core cyber-range boundary.

Where used, these services are treated as external supporting services with controlled information flows from the cyber range to the monitoring environment.

Their inclusion may be reconsidered in later labs if monitoring or detection becomes a required system function.

## Network Isolation

The cyber range is intended to operate on an isolated virtual network that is not directly connected to the normal home LAN.

The target system should have no direct connectivity to the home LAN or other production/home systems.

If the Kali attacker VM requires external connectivity for administrative tasks, software updates, or research, that connectivity must be provided through a separate interface and must not create an unintended path between the isolated cyber-range network and the home LAN.

## Trust Boundaries

Primary trust boundaries include:

1. the boundary between the isolated cyber range and the Proxmox hosting environment
2. the boundary between the Proxmox management plane and the cyber-range VMs
3. the boundary between any external monitoring services and the cyber range
4. the boundary between any externally connected Kali interface and the isolated attack network

## Assumptions

- IP forwarding or bridging between the isolated cyber-range network and the home LAN is not enabled.
- The target VM remains accessible only through the isolated range network unless explicitly required for a future lab.
- Administrative access to Proxmox remains separate from cyber-range attack traffic.
- External monitoring connections, if implemented, are controlled and do not provide an inbound path into the range.

## Boundary Rationale

The boundary is intentionally limited to the attacker, target, and isolated network required to perform cyber-range exercises.

This narrow boundary keeps the governed system understandable, reduces unnecessary scope, and makes later control selection and assessment more defensible.

Supporting infrastructure remains documented as an external dependency rather than being automatically included in the system boundary.

## Isolation Validation

Host-side isolation of `vmbr1` was directly validated on the Proxmox platform.

The active Proxmox network configuration defines:

- `vmbr1` as an interface in manual mode
- `bridge-ports none`
- no host-side IPv4 address on `vmbr1`
- no route through `vmbr1` in the Proxmox routing table

The Proxmox host's routed connectivity is provided through `vmbr0`, while `vmbr1` has no configured physical uplink.

This evidence supports the boundary decision that `vmbr1` functions as an isolated Layer-2 cyber-range network.

The validation does not eliminate the need to reassess isolation if a cyber-range VM becomes multi-homed or guest-level forwarding is enabled.

See: `../evidence/EV-001-vmbr1-host-isolation.txt`
