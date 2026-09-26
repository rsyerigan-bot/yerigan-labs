# GRC-002 — Information Types

## Purpose

Identify the information created, stored, processed, or transmitted by the Yerigan Labs Cyber Range before assigning confidentiality, integrity, or availability impact levels.

This inventory supports a practice system categorization based on confidentiality, integrity, and availability impact concepts.

## Information Types

| ID | Information Type | Examples | Current Use | Primary Concern |
|---|---|---|---|---|
| IT-001 | System configuration information | VM operating-system settings, network addressing, service configuration | Stored and processed within cyber-range systems; some virtualization configuration is administered through external Proxmox infrastructure | Integrity |
| IT-002 | Assessment results | host-discovery output, port scans, service enumeration | Created and processed during authorized security exercises | Integrity |
| IT-003 | Vulnerability findings | suspected vulnerabilities, validation results, remediation observations | Created during vulnerability-assessment exercises | Integrity |
| IT-004 | Exploitation evidence | command output, proof of execution, before/after observations | Created during controlled exploitation exercises | Integrity |
| IT-005 | Logs and telemetry | local operating-system logs, service logs, security events | Local logging may exist within range systems; centralized telemetry transmission remains unverified | Integrity |
| IT-006 | Administrative information | usernames, system-management details, configuration references | Used to administer the training environment; the range is not intended to contain production credentials or sensitive household information | Integrity / Confidentiality |

## Confidentiality Considerations

The cyber range is not intended to contain:

- personal information
- employer or production information
- NAS data
- household or IoT data
- production credentials
- sensitive business information

Disclosure of lab configurations, scan results, or training findings could reveal details about the environment, but the system is intentionally non-production and isolated.

## Integrity Considerations

Integrity is particularly important because unauthorized modification could alter:

- system configuration
- vulnerable services
- network behavior
- assessment results
- exploitation evidence
- administrative access

Such changes could cause inaccurate conclusions or invalidate training exercises.

## Availability Considerations

The information and systems support cybersecurity training and professional development.

Loss would interrupt exercises and may require rebuilding or repeating work, but no production, safety-critical, or mission-critical function currently depends on the cyber range.

## Recovery Considerations

The environment is supported by:

- Git history
- lab documentation
- engineering journals
- runbooks
- architecture documentation

These artifacts reduce the difficulty of reconstructing the environment following loss.

## Assumptions and Limitations

- The range is assumed to remain isolated from the normal home network.
- The range is not intended to contain sensitive production or household information.
- Centralized monitoring and logging connectivity has not yet been validated.
- This analysis must be revisited if the system purpose, connectivity, information types, or dependencies materially change.

## Status

Information types reviewed and documented for GRC-002.
