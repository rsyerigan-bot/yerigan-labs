# GRC-001 — System Boundary and Asset Inventory

## Objective

Define the security boundary for the Yerigan Labs Cyber Range and create an initial asset inventory for the systems and components included within that boundary.

This lab establishes the foundation required for later governance work such as:

- system categorization
- control selection
- control implementation statements
- evidence collection
- security assessment
- findings management
- risk documentation
- POA&M development
- continuous monitoring

## Learning Goal

Demonstrate that security governance begins with understanding exactly what system is being governed.

Before assessing controls or discussing risk, the system boundary, components, interfaces, dependencies, and administrative relationships must be clearly defined.

## System

**Name:** Yerigan Labs Cyber Range

**Purpose:**
Provide an isolated, authorized environment for cybersecurity training, network reconnaissance, vulnerability validation, and controlled exploitation exercises.

## Scope

This lab focuses only on defining the cyber-range system boundary and identifying its assets.

No security-control assessment is performed in GRC-001.

## Boundary Summary

In-scope components:

- Kali Linux VM 101
- Target VM 102
- isolated `vmbr1` cyber-range network

Out-of-scope supporting components:

- physical Proxmox host
- Proxmox management interface
- Windows administrative workstation
- centralized logging or monitoring services, if used

The cyber-range boundary is intentionally limited to the attacker, target, and isolated virtual network required to perform authorized training exercises.

## Deliverables

- documented system purpose
- defined system boundary
- list of in-scope components
- list of out-of-scope components
- external dependencies
- administrative interfaces
- asset inventory
- boundary assumptions
- unresolved questions

## Status

Complete.

The cyber-range system boundary, in-scope and supporting assets, administrative relationships, data flows, network isolation, and current addressing state have been documented and validated.

Remaining open items represent future configuration decisions rather than unresolved system-boundary questions.
