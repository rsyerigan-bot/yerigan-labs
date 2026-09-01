# Documentation Debt Register

## Purpose

Track missing or intentionally deferred Yerigan Labs documentation so engineering work remains reproducible, auditable, and useful for operations, interviews, future employees, and future clients.

Documentation debt should trend toward zero.

---

## Open Documentation Debt

| Priority | Lab | Missing Artifact | Status | Notes |
|---|---|---|---|---|
| High | Security | Router Perimeter Review | Deferred | GFiber management unavailable; review port forwarding, UPnP, IPv6 firewall, DMZ, and remote administration |
| High | Security | External Exposure Validation | Open | Independently validate externally reachable services |
| Medium | Security | Home Assistant 8123 Review | Deferred | Determine whether direct LAN access remains required after permanent network deployment |
| High | Security | Permanent Network Baseline | Deferred | Reassess security controls after move and deployment of permanent router/network |
| Medium | Monitoring | Additional Host Alerts | Deferred | CPU and memory alert policies intentionally excluded from LAB-010 |
| Medium | Monitoring | Incident Context Capture | Deferred | Capture system context at alert firing and recovery |
| Medium | Monitoring | Monitoring Baseline Review | Deferred | Establish normal network and resource baselines after sufficient historical data exists |
| Medium | Logging | UFW Log Collection | Deferred | Add host firewall events as a separate evidence source |
| Medium | Logging | Caddy Access Logging | Deferred | Add HTTP access evidence after centralized logging foundation is validated |
| Medium | Logging | Logging Capacity Review | Deferred | Review Loki storage growth and 30-day retention after sufficient historical data exists |
---

## Rules

1. Missing required documentation must be recorded here.
2. Documentation debt should be reduced during normal engineering work.
3. New labs should not create undocumented debt without recording it here.
4. Not every lab requires every artifact; applicability must be explicitly reviewed.
5. Completed items should be removed from Open Documentation Debt and recorded below.

---

## Completed Documentation Debt

| Lab | Artifact | Completed | Notes |
|---|---|---|---|
| LAB-006 | Engineering Journal | Yes | Infrastructure-as-Code migration documented |
| LAB-007 | Engineering Journal | Yes | Home Platform foundation documented |
| LAB-001–007 | OI Applicability Review | Yes | Reviewed; no additional OIs required for these labs |
| LAB-002 | Runbook Applicability Review | Yes | Reviewed; no recurring operational recovery procedure required |
| LAB-005 | Runbook Applicability Review | Yes | Reviewed; credential recovery covered by RB-009; backup/restore remains separate deferred engineering work |
| LAB-006 | Runbook Applicability Review | Yes | Reviewed; no recurring operational recovery procedure required |
| LAB-007 | Runbook Applicability Review | Yes | Reviewed; no recurring operational recovery procedure required |
| Framework | Interview-Notes Template | Yes | Template created for future and retrospective interview documentation |
| Framework | Validation-Checklist Template | Yes | Template created for repeatable validation documentation |
| LAB-003 | Security / Firewall Verification & Recovery Runbook | Yes | RB-003 created for evidence-based firewall and service-exposure troubleshooting |
| LAB-004 | Caddy Troubleshooting & Recovery Runbook | Yes | RB-004 created for layered reverse-proxy troubleshooting and recovery |
| LAB-001 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-002 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-003 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-004 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-005 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-006 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-007 | Interview Notes | Yes | Retrospective interview notes created from available project evidence |
| LAB-008 | Interview Notes | Yes | Existing abbreviated notes normalized to current retrospective interview-note standard |
| LAB-009 | Interview Notes | Yes | Retrospective interview notes created from presence-confidence evidence and documented limitations |
| LAB-010 | Interview Notes | Yes | Retrospective interview notes created from observability, LVM, alerting, and notification evidence |
| LAB-011 | Interview Notes | Yes | Retrospective interview notes created from centralized logging and ingestion-validation evidence |
| LAB-012 | Interview Notes | Yes | Retrospective interview notes created from authorized vulnerability-validation evidence |
| LAB-008–012 | Runbook Applicability Review | Yes | Reviewed; existing operational runbooks retained where recurring procedures exist; no additional runbook required for LAB-009 or LAB-012 |
| LAB-008–012 | OI Applicability Review | Yes | Reviewed; OI-008 remains useful; additional OIs would duplicate engineering or validation documentation |
| LAB-008–012 | Validation Artifact Review | Yes | Existing tickets and engineering records contain sufficient validation evidence; standalone retrospective validation checklists not required |
| LAB-008–012 | Documentation Mapping Review | Yes | Artifact relationships reviewed by project content rather than assuming LAB and ENG sequence numbers match |
