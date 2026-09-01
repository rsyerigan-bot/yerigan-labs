# Yerigan Labs Engineering Roadmap

## Purpose

Yerigan Labs is a practical family technology platform, engineering lab, and professional-development environment.

Projects should provide one or more of the following:

1. useful family capability
2. improved security or privacy
3. improved reliability or recoverability
4. useful automation
5. technical education and portfolio value
6. operational or future business value

The roadmap tracks **capabilities**, not merely technologies.

A technology should not be added only because it is common in enterprise environments. New work should solve a defined problem, reduce a meaningful risk, or develop a deliberate engineering skill.

---

## Engineering Rhythm

Yerigan Labs should rotate between different types of engineering work rather than becoming exclusively infrastructure, automation, or cybersecurity focused.

Preferred rhythm:

**Build → Secure → Use → Attack → Detect → Improve**

Not every project must include every stage.

Projects should remain scoped, validated, documented, and useful.

---

# 1. Platform Foundation

## Current Capability

- [x] Ubuntu Server platform
- [x] Docker container platform
- [x] Git and GitHub version control
- [x] Infrastructure configuration migrated toward Git source of truth
- [x] Caddy reverse-proxy foundation
- [x] Friendly internal service names
- [x] Homepage service dashboard
- [x] Home Assistant platform
- [x] Engineering documentation framework
- [x] Documentation-debt tracking
- [x] Runbook and interview-note framework

## Future Improvements

- [ ] Review container image version-pinning strategy
- [ ] Evaluate deployment/configuration automation when environment scale justifies it
- [ ] Continue reducing unmanaged server-local configuration

---

# 2. Family Platform & Automation

## Current Capability

- [x] Home Assistant foundation
- [x] Mobile notification framework
- [x] Event-driven automation foundation
- [x] Initial smart-device integration
- [x] Presence-confidence design and evidence evaluation
- [x] Family-facing and operations-dashboard design principles

## Presence Roadmap

Current presence work demonstrated that individual signals may be stale, missing, or unreliable.

Phone-side Wi-Fi/SSID evidence may be useful corroborating evidence but should not be treated as authoritative presence truth under the current architecture.

Future work:

- [ ] Add reliable network-side presence evidence after network modernization
- [ ] Evaluate motion/occupancy evidence where useful
- [ ] Define signal freshness requirements
- [ ] Define confidence behavior for missing and contradictory evidence
- [ ] Add presence-driven automations only after evidence quality is sufficient

AI-based presence detection is not currently a priority and should be added only if it solves a defined household problem.

## Family Automation

Potential future work:

- [ ] useful lighting automation
- [ ] household status notifications
- [ ] security-related notifications
- [ ] energy monitoring and automation
- [ ] family-friendly service dashboards
- [ ] additional local-first smart-home integrations

Automations should provide practical household value rather than exist only as demonstrations.

---

# 3. Security & Identity

## Current Capability

- [x] SSH public-key authentication
- [x] SSH password authentication disabled
- [x] SSH root login disabled
- [x] UFW host-firewall baseline
- [x] reduced Docker host-port exposure
- [x] Caddy centralized web entry point
- [x] repository secrets review
- [x] runtime secrets excluded from Git
- [x] password-manager strategy established
- [x] documented security baseline

## Near-Term Priority — Identity and Administrative Access

- [ ] inventory administrative identities and authentication methods
- [ ] classify password, SSH-key, MFA, passkey, and recovery mechanisms
- [ ] enable MFA where supported and appropriate
- [ ] evaluate passkeys and hardware security keys based on actual risk
- [ ] define administrative-account standards
- [ ] define credential-recovery expectations
- [ ] distinguish normal-use and administrative access where useful

Hardware security keys should be adopted only where they materially improve security or recovery rather than as a technology demonstration.

## Deferred / Network-Dependent Security Work

- [ ] router perimeter review
- [ ] independent external-exposure validation
- [ ] permanent network security baseline
- [ ] review direct Home Assistant TCP/8123 LAN access
- [ ] review IPv6 exposure and firewall behavior
- [ ] review UPnP, port forwarding, DMZ, and router remote administration

These items should be reassessed when the permanent network equipment is deployed.

---

# 4. Observability, Logging & Detection

## Current Capability

- [x] Node Exporter host telemetry
- [x] cAdvisor container telemetry
- [x] Prometheus metrics collection
- [x] Grafana visualization
- [x] filesystem-capacity alerting
- [x] alert delivery through Caddy and Home Assistant
- [x] centralized Ubuntu journal collection
- [x] centralized Docker log collection
- [x] Grafana Alloy collection
- [x] Loki log storage
- [x] Grafana log investigation
- [x] persistent log-collector state
- [x] dedicated logging storage
- [x] initial 30-day logging retention

## Monitoring Improvements

- [ ] CPU alert policy
- [ ] memory alert policy
- [ ] incident-context capture
- [ ] establish normal resource baselines
- [ ] capacity and trend review
- [ ] evaluate additional service-health signals

## Logging Improvements

- [ ] UFW log collection
- [ ] Caddy access logging
- [ ] Loki capacity review
- [ ] retention review using observed storage growth
- [ ] ingestion-health monitoring

## Detection Engineering

Centralized logging provides evidence but is not yet a complete detection or SIEM capability.

Future work:

- [ ] define useful security detections
- [ ] failed-authentication detection
- [ ] suspicious administrative activity detection
- [ ] firewall-event analysis
- [ ] web-access anomaly investigation
- [ ] correlate multiple evidence sources
- [ ] create repeatable incident-investigation workflow

Automated remediation should not be introduced until detection quality and failure behavior are understood.

---

# 5. Backup, Recovery & Resilience

## Current State

Backup and recovery requirements have been identified in earlier engineering work, but a complete backup and restore capability has **not yet been implemented**.

This is a priority infrastructure gap.

## Near-Term Priority — Backup & Recovery Foundation

- [ ] inventory data and configuration requiring backup
- [ ] classify configuration versus runtime state
- [ ] define recovery priorities
- [ ] define acceptable data-loss expectations
- [ ] implement Home Assistant backup
- [ ] implement critical Ubuntu/service-state backup
- [ ] establish a local backup target
- [ ] integrate future NAS storage when available
- [ ] document restoration procedures
- [ ] perform controlled restore validation

## Future Resilience

- [ ] Proxmox backup strategy
- [ ] NAS backup strategy
- [ ] off-site copy for irreplaceable data where justified
- [ ] backup integrity verification
- [ ] periodic restore testing
- [ ] UPS integration
- [ ] graceful shutdown behavior
- [ ] recovery-priority documentation
- [ ] disaster-recovery exercise

A backup should not be considered reliable until restoration has been tested.

---

# 6. Network Modernization

## Current State

The current network supports the lab but limits segmentation, authoritative network-side presence evidence, and some perimeter validation.

Major redesign should wait for permanent network equipment rather than building temporary complexity that will be discarded.

## Future Capability

- [ ] deploy permanent router/firewall
- [ ] document physical and logical topology
- [ ] trusted-client network
- [ ] server/infrastructure network
- [ ] IoT network
- [ ] security/camera network if required
- [ ] guest network
- [ ] management-access strategy
- [ ] inter-VLAN firewall policy
- [ ] DNS architecture review
- [ ] DHCP architecture review
- [ ] IPv6 security review
- [ ] authoritative network-side presence evidence
- [ ] external-exposure validation

Segmentation should be driven by trust boundaries and operational requirements rather than VLAN count.

---

# 7. Cyber Range & Security Engineering

## Completed Range Foundation

- [x] RED-001 — Isolated Cyber Range
- [x] RED-002 — Host Discovery and Network Reconnaissance
- [x] RED-003 — Port Scanning and Service Discovery
- [x] RED-004 — Vulnerability Validation and Controlled Exploitation

The range is intentionally isolated from the household LAN and is used only for authorized security education.

## Skills Demonstrated

- scope definition
- network isolation
- host discovery
- service enumeration
- vulnerability research
- evidence validation
- controlled exploitation
- exploit-versus-payload reasoning
- impact validation
- stopping-point discipline
- post-test validation
- security finding documentation

## Future Security Labs

Potential work:

- [ ] defender-side investigation of known range activity
- [ ] detection engineering from controlled attack evidence
- [ ] authentication attack/detection concepts in an isolated environment
- [ ] web-security fundamentals
- [ ] vulnerability remediation and revalidation
- [ ] additional controlled exploitation concepts
- [ ] OSINT/privacy self-assessment and personal-data reduction

Cyber-range work should remain one branch of Yerigan Labs rather than displacing family, infrastructure, networking, and operational projects.

---

# 8. Operational & Future Business Maturity

## Documentation and Operations

- [x] engineering journals
- [x] lab tickets
- [x] architecture documentation
- [x] engineering decision documentation
- [x] runbook framework
- [x] operational-instruction framework
- [x] interview-note framework
- [x] validation-checklist framework
- [x] documentation-debt process
- [x] evidence-first troubleshooting principles

## Future Operational Capability

- [ ] asset inventory
- [ ] service ownership
- [ ] dependency mapping
- [ ] patch-management process
- [ ] recurring vulnerability-management lifecycle
- [ ] change-management maturity
- [ ] configuration-baseline review
- [ ] incident-response workflow
- [ ] service recovery objectives
- [ ] periodic operational review

## Future Enterprise-Learning Projects

These should be implemented when they teach a useful concept or support another project, not simply to imitate enterprise complexity.

- [ ] centralized identity / directory-service concepts
- [ ] PKI and certificate lifecycle
- [ ] secrets-management platform
- [ ] SIEM concepts after logging and detection maturity justify it
- [ ] vulnerability-management lifecycle
- [ ] endpoint-management concepts
- [ ] policy and control mapping
- [ ] risk-register practice
- [ ] service-management / ITIL concepts

RED-004 demonstrated vulnerability **validation**. It does not represent a complete vulnerability-management program.

---

# Near-Term Engineering Queue

The exact order may change when dependencies or family priorities change.

## Priority 1 — Identity & Administrative Access Hardening

Improve administrative authentication, MFA, recovery, and credential governance across the existing platform.

## Priority 2 — Backup & Recovery Foundation

Protect the infrastructure and household platform that now depend on persistent configuration and runtime state.

## Priority 3 — Family-Useful Capability

Build or improve a service or automation with direct household value.

## Priority 4 — Detection / Investigation

Use existing monitoring, logging, and controlled cyber-range activity to practice evidence-driven investigation.

## Priority 5 — Cyber-Range Expansion

Continue security training with a deliberately scoped exercise.

## Priority 6 — Permanent Network Modernization

After permanent network equipment is available, establish segmentation, perimeter controls, authoritative network evidence, and the permanent security baseline.

---

# Roadmap Principles

1. Family usefulness and operational reliability matter as much as technical novelty.
2. Do not introduce enterprise complexity without a reason.
3. Prefer local control and low recurring cost where practical.
4. Security controls must be validated rather than assumed.
5. Monitoring is not the same as logging.
6. Logging is not the same as detection.
7. Vulnerability validation is not the same as vulnerability management.
8. Backup configuration is not the same as validated recovery.
9. A running container does not prove a healthy service.
10. A documented design does not prove implementation.
11. Deferred work should remain visible rather than being retroactively marked complete.
12. Cyber-range activity must remain isolated and explicitly authorized.
13. Build capabilities that can be explained, operated, recovered, and improved.
