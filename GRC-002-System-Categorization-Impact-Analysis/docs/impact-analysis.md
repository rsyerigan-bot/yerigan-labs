# GRC-002 — Impact Analysis

## Purpose

Evaluate the potential consequences of losing confidentiality, integrity, or availability within the Yerigan Labs Cyber Range.

The analysis is limited to the system boundary established in GRC-001.

Potential compromise of systems outside that boundary, such as the normal home network, NAS, IoT devices, or other household systems, is treated as a separate risk scenario rather than as information inherently contained within the cyber range.

## Confidentiality

### Observations

The cyber range is not intended to store or process sensitive personal, employment, production, or household information.

Kali 101 is a dedicated assessment VM used for authorized testing against intentionally vulnerable training systems.

The range is not intended to contain:

- personal information
- employer information
- production-system information
- NAS data
- IoT data
- sensitive household information
- production credentials

Technical information such as system configuration, scan results, service information, and vulnerability findings may exist within the range.

### Potential Consequence of Loss

Unauthorized disclosure of cyber-range information could expose details about the lab configuration and testing methodology.

However, because the environment is intentionally isolated, non-production, and designed for training, disclosure of this information would create limited adverse impact.

### Proposed Confidentiality Impact

**LOW**

## Integrity

### Observations

Integrity is important to the reliability of the cyber range.

Unauthorized changes could include:

- modification of VM configurations
- alteration of vulnerable services
- changes to administrator credentials
- removal of administrator access
- modification of assessment evidence
- changes to network configuration
- alteration of system state before or during testing

These changes could cause test results to become inaccurate or misleading.

A compromised configuration could lead to incorrect conclusions regarding vulnerabilities, controls, or system behavior.

### Potential Consequence of Loss

Loss of integrity could invalidate training exercises, assessment results, or collected evidence.

However, the environment is non-production and can be reconstructed using existing documentation, lab records, and configuration information.

The impact would therefore be limited primarily to lost time, repeated work, and reduced confidence in previous results.

### Proposed Integrity Impact

**LOW**

### Operational Note

Although the proposed impact level is Low, integrity is the most important security objective for the cyber range's intended function.

The value of the environment depends on being able to trust that configuration, evidence, and observed system behavior accurately represent the state being tested.

## Availability

### Observations

The cyber range is a training and professional-development environment.

It does not currently provide a mission-critical, production, safety, or household service.

Loss of Kali 101, Target 102, or the entire range would temporarily prevent cybersecurity exercises.

### Recovery Considerations

The environment is documented through:

- lab documentation
- engineering journals
- runbooks
- architecture documentation
- Git history

Because of this documentation and the relatively small system scope, the cyber range could likely be reconstructed without significant long-term impact.

### Potential Consequence of Loss

Loss of availability would interrupt training and require recovery or reconstruction effort.

No critical operational or business function currently depends on the cyber range.

### Proposed Availability Impact

**LOW**

## Preliminary System Categorization

Based on the current system purpose, information types, and potential consequences of loss:

| Security Objective | Proposed Impact | Rationale |
|---|---|---|
| Confidentiality | Low | The range does not intentionally contain sensitive or production information |
| Integrity | Low | Modification could invalidate testing, but the training environment is recoverable |
| Availability | Low | Outage interrupts training but does not affect a critical operational function |

## Security Objective Priority

For the intended function of the cyber range:

**Integrity is the most important operational security objective.**

This does not increase the integrity impact rating by itself.

Operational priority describes which property is most important to reliable system use, while impact categorization describes the severity of consequences if that property is lost.

## Boundary-Related Risk

A compromise that enables access from the cyber range into the normal home network could expose systems and information with substantially greater confidentiality, integrity, or availability requirements.

That scenario is not used to increase the cyber range's current system-impact categorization.

Instead, it will be evaluated separately as a threat and risk scenario during later risk-assessment work.

## Preliminary Conclusion

Proposed system categorization:

- **Confidentiality:** LOW
- **Integrity:** LOW
- **Availability:** LOW

Overall impact remains Low based on the currently defined system purpose and boundary.

This conclusion should be revisited if the cyber range later stores sensitive information, connects to production or household systems, becomes operationally necessary, or begins supporting functions with greater consequences of loss.

## Integrity Scenario Validation

A hypothetical integrity-loss scenario was considered in which Target 102 is silently modified so that an expected vulnerability is no longer present.

Such a change could cause an assessment to produce an incorrect conclusion if the modification were not detected.

The integrity impact remains **Low** because the consequences are limited to inaccurate training results, lost time, repeated testing, and rework within a non-production environment.

The ability to troubleshoot, detect unexpected behavior, or reconstruct the environment may improve recovery, but those factors are not the primary basis for the impact rating.

Impact categorization is based on the consequence of losing integrity, not on the assumed likelihood of detecting or correcting the problem.
