# Yerigan Labs

Yerigan Labs is my hands-on cybersecurity, infrastructure, and security-governance learning environment.

I use it to build, secure, observe, test, troubleshoot, and increasingly assess systems through structured labs and documented engineering work.

The goal is not to collect tools or reproduce tutorials. Each project is built around a specific objective and emphasizes direct validation, troubleshooting, documentation, and a clear distinction between what was observed, what was demonstrated, and what remains unproven.

My long-term focus is cybersecurity governance, risk, and security oversight. Yerigan Labs provides the technical foundation needed to understand the systems, evidence, vulnerabilities, and operational realities behind those responsibilities.

## Current Focus

The environment has progressed beyond basic homelab setup into:

- secure systems administration
- infrastructure monitoring and logging
- network troubleshooting
- vulnerability assessment
- controlled offensive-security exercises
- evidence-based validation
- security documentation
- governance-oriented security work

Current development areas include:

- network segmentation and packet analysis
- defensive monitoring and investigation
- vulnerability remediation and revalidation
- system boundaries and asset inventories
- security control implementation and assessment
- risk documentation and POA&M workflows
- continuous monitoring concepts

## What This Repository Demonstrates

### Systems and Infrastructure

- Proxmox VE
- Ubuntu Server
- Docker and Docker Compose
- Caddy reverse proxy
- Pi-hole
- Home Assistant
- Git-based configuration management
- LVM storage management
- service troubleshooting
- infrastructure documentation

### Security Hardening

- SSH public-key authentication
- disabled remote password authentication
- disabled remote root login
- host firewall configuration
- service-exposure reduction
- secrets-handling practices
- security baseline documentation

### Monitoring and Logging

- Prometheus
- Grafana
- Node Exporter
- cAdvisor
- Grafana Alloy
- Loki
- host and container log collection
- alert validation
- storage and retention planning

Monitoring identified a real capacity issue when the Ubuntu root filesystem reached approximately 99% utilization. I validated the condition, investigated the LVM layout, expanded the root filesystem using available capacity, and confirmed recovery.

### Networking

- static IPv4 addressing
- ARP and ICMP validation
- isolated virtual networks
- host discovery
- TCP port-state interpretation
- service and version enumeration
- DNS troubleshooting
- permanent-network migration
- Pi-hole filtering and allow-list troubleshooting

### Security Assessment

- authorized scope definition
- isolated cyber-range design
- reconnaissance and enumeration
- vulnerability research
- evidence-led vulnerability validation
- controlled exploitation
- independent verification of tool output
- demonstrated-impact versus potential-impact analysis
- post-test validation and cleanup

## Selected Security Labs

### RED-001 — Isolated Cyber Range

Built an isolated Proxmox-based security lab using Kali Linux and Metasploitable 2.

The project established the authorized testing environment before offensive-security activity began, including network isolation, target validation, addressing, and route verification.

[`RED-001-Isolated-Cyber-Range/`](./RED-001-Isolated-Cyber-Range/)

### RED-002 — Host Discovery and Network Reconnaissance

Compared host-discovery techniques inside the authorized cyber range and validated discovered systems before proceeding to deeper enumeration.

[`RED-002-Host-Discovery-Network-Reconnaissance/`](./RED-002-Host-Discovery-Network-Reconnaissance/)

### RED-003 — Port Scanning and Service Discovery

Progressed from host discovery into TCP port-state analysis and service/version enumeration while distinguishing observations from conclusions.

[`RED-003-Port-Scanning-Service-Discovery/`](./RED-003-Port-Scanning-Service-Discovery/)

### RED-004 — Vulnerability Validation and Controlled Exploitation

Investigated a suspected vulnerability affecting vsftpd 2.3.4 and treated the version identification as a hypothesis rather than proof.

The assessment established pre-test network state, researched the suspected vulnerability, independently validated framework behavior, demonstrated unauthorized root-level command execution on the intended training target, stopped after sufficient impact was proven, and verified the temporary listener was closed afterward.

Persistence, credential collection, lateral movement, destructive activity, and unnecessary post-exploitation were intentionally excluded.

[`RED-004-Vulnerability-Validation-Controlled-Exploitation/`](./RED-004-Vulnerability-Validation-Controlled-Exploitation/)

## Engineering and Documentation

Documentation is part of the work, not an afterthought.

The repository includes:

- lab tickets
- engineering journals
- architecture documentation
- engineering decisions
- runbooks
- operational instructions
- interview notes
- documentation-debt tracking
- Git-based change history

## How I Approach Labs

A tool result is evidence, not automatically a conclusion.

My general workflow is:

**Define scope → Establish baseline → Observe → Validate → Investigate → Demonstrate → Document → Identify limitations**

For portfolio purposes, I try to show that I can:

1. explain what I am testing and why
2. perform the work safely
3. validate results independently where practical
4. troubleshoot unexpected behavior
5. distinguish evidence from assumptions
6. identify limitations
7. document the result clearly enough for another person to review

## Repository Structure

- `design/` — Architecture, principles, standards, and target-state planning
- `docs/architecture/` — Current system and network architecture
- `docs/engineering-decisions/` — Decisions, alternatives, and trade-offs
- `docs/engineering-journal/` — Implementation notes and lessons learned
- `docs/interview-notes/` — Career-focused explanations and discussion points
- `docs/operations/` — Operational instructions
- `docs/runbooks/` — Repeatable operating and recovery procedures
- `docs/standards/` — Engineering and security standards
- `docs/Tickets/` — Structured lab records
- `infrastructure/docker/` — Docker Compose and service configuration
- `scripts/` — Administration and automation scripts
- `RED-*` — Cyber-range and security-assessment labs

## Next Phase

The next phase of Yerigan Labs will increasingly connect technical implementation to cybersecurity governance and risk.

Planned work includes:

- system boundary definition
- asset inventory
- secure network architecture
- packet analysis
- defensive log investigation
- detection fundamentals
- vulnerability remediation and revalidation
- security control implementation statements
- evidence collection and control testing
- risk registers
- findings management
- POA&M workflows
- continuous monitoring

The objective is to understand not only how a system is built or attacked, but also:

- what is being protected
- what controls should exist
- how those controls can be verified
- what evidence supports a security conclusion
- what risk remains
- how that risk should be communicated

## Scope and Ethics

Security testing documented in this repository is performed only against systems I own or intentionally vulnerable training environments that I am authorized to test.

Examples involving exploitation are conducted inside isolated lab environments.

Successful access does not expand authorization.

Testing stops when sufficient evidence has been collected to satisfy the stated objective.

## About Me

I am building toward cybersecurity governance, risk, and security-oversight roles with a technical foundation in enterprise IT operations, networking, systems administration, and hands-on security work.

My background has made evidence, documentation, repeatability, quality control, and disciplined troubleshooting recurring themes in how I approach technical problems.

Yerigan Labs documents that development through work I can explain, reproduce, defend, and continue improving.
