# Documentation Debt Register

## Purpose

Track missing or intentionally deferred Yerigan Labs documentation so engineering work remains reproducible, auditable, and useful for operations, interviews, future employees, and future clients.

Documentation debt should trend toward zero.

---

## Open Documentation Debt

| Priority | Lab | Missing Artifact | Status | Notes |
|---|---|---|---|---|
| Medium | LAB-001 | Interview Notes | Open | Backfill |
| Medium | LAB-002 | Interview Notes | Open | Backfill |
| Medium | LAB-003 | Interview Notes | Open | Backfill |
| Medium | LAB-004 | Interview Notes | Open | Backfill |
| Medium | LAB-005 | Interview Notes | Open | Backfill |
| Medium | LAB-006 | Interview Notes | Open | Backfill |
| Medium | LAB-007 | Interview Notes | Open | Backfill |
| Medium | LAB-001 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-002 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-003 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-004 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-005 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-006 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-007 | OI Review | Open | Determine whether an OI is required |
| Medium | LAB-002 | Runbook Review | Open | Determine operational recovery need |
| Medium | LAB-003 | Runbook Review | Open | Determine operational recovery need |
| Medium | LAB-004 | Runbook Review | Open | Determine operational recovery need |
| Medium | LAB-005 | Runbook Review | Open | Determine operational recovery need |
| Medium | LAB-006 | Runbook Review | Open | Determine operational recovery need |
| Medium | LAB-007 | Runbook Review | Open | Determine operational recovery need |
| Medium | Framework | Interview-Notes Template | Open | Planned template was not created |
| Medium | Framework | Validation-Checklist Template | Open | Planned template was not created |
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

Completed Documentation Debt

| Lab | Artifact | Completed | Notes |
|---|---|---|---|
| LAB-006 | Engineering Journal | Yes | Infrastructure-as-Code migration documented |
| LAB-007 | Engineering Journal | Yes | Home Platform foundation documented |
