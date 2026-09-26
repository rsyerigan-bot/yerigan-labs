# GRC-002 — System Categorization

## System

**System Name:** Yerigan Labs Cyber Range

**System Purpose:**
Provide an isolated and authorized environment for cybersecurity training, network reconnaissance, vulnerability validation, and controlled exploitation.

## Categorization Approach

This exercise applies confidentiality, integrity, and availability impact concepts to the system boundary established in GRC-001.

The ratings represent the potential adverse impact resulting from loss of each security objective.

This is a practice categorization for a personal training environment and is not represented as a formal federal system categorization.

## Confidentiality

**Impact: LOW**

The cyber range is not intended to contain sensitive personal, production, employer, NAS, IoT, or household information.

Unauthorized disclosure could expose lab configurations, scan results, vulnerability information, or training methodology, but the resulting harm would be limited because the environment is isolated and non-production.

## Integrity

**Impact: LOW**

Unauthorized modification could:

- alter VM or network configuration
- modify vulnerable services
- change administrative access
- corrupt assessment results
- alter exploitation evidence
- produce inaccurate security conclusions

Integrity is the most important operational security objective for the cyber range because reliable training depends on trustworthy system state and evidence.

Despite that importance, the impact remains Low because consequences are limited primarily to inaccurate training results, lost time, repeated testing, and reconstruction of a non-production environment.

## Availability

**Impact: LOW**

Loss of the cyber range would interrupt cybersecurity exercises and professional-development work.

No production, business-critical, safety-critical, or household function currently depends on the range.

Existing documentation, Git history, architecture records, and runbooks also support reconstruction.

## System Security Category

The proposed security category is:

**SC = {(Confidentiality, LOW), (Integrity, LOW), (Availability, LOW)}**

Therefore:

**Overall System Impact: LOW**

## Operational Priority

Although all three impact ratings are Low, they are not equally important to the system's intended use.

**Integrity is the highest operational priority.**

This distinction does not change the impact rating.

Operational priority describes which security property is most important to reliable system use.

Impact categorization describes the severity of the consequences if that property is lost.

## Considered Scenario

A scenario was evaluated in which Target 102 is silently modified so that an expected vulnerability is no longer present.

If undetected, the change could lead to an incorrect assessment conclusion.

Integrity remains Low because the consequences are limited to training accuracy, lost time, repeated testing, and rework within a recoverable non-production environment.

The likelihood of detecting the change or the ability to troubleshoot it is not used as the primary basis for the impact determination.

## Boundary-Related Risk

Compromise of the cyber range could become substantially more consequential if an attacker gained access to systems outside the defined boundary, including:

- the normal home network
- NAS resources
- IoT devices
- household endpoints
- other trusted systems

That possibility does not increase the current system-impact rating by itself.

It represents a threat scenario involving boundary failure and will be evaluated during the subsequent risk-assessment lab.

## Reassessment Triggers

The categorization should be reviewed if:

- sensitive information is introduced
- production credentials are stored
- the cyber range gains persistent connectivity to trusted networks
- the range begins supporting operational services
- recovery becomes materially more difficult
- other users or organizational stakeholders begin depending on the environment
- the system boundary materially changes

## Conclusion

The Yerigan Labs Cyber Range is categorized as:

**LOW / LOW / LOW**

The system's limited information sensitivity, non-production purpose, narrow scope, and recoverability support a Low overall impact determination.

Integrity remains the most important security objective for maintaining reliable and defensible training results.
