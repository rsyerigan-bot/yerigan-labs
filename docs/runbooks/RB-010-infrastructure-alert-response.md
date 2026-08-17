# RB-010 — Infrastructure Alert Response

## Purpose

Provide a repeatable initial response when Yerigan Labs infrastructure monitoring generates a warning or critical alert.

## Initial Response

When an infrastructure alert is received:

1. Read the alert severity, service, category, and description.
2. Do not immediately restart the host or affected service unless required to restore a critical family or safety function.
3. Open Grafana and review the affected metric.
4. Review related host-health metrics for the same time period.
5. Validate the reported condition independently from the Ubuntu host.
6. Preserve relevant evidence before making changes.
7. Identify the likely root cause.
8. Perform the least disruptive appropriate remediation.
9. Verify recovery using both host-level tools and monitoring telemetry.
10. Record significant findings or corrective actions in the appropriate engineering documentation.

## Root Filesystem Capacity

For a filesystem-capacity alert, begin with:

    df -hT /
    df -ih /
    sudo du -xhd1 / 2>/dev/null | sort -h
    docker system df

If storage allocation may be involved:

    lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINTS
    sudo pvs
    sudo vgs
    sudo lvs

Do not delete data or run broad cleanup commands until the source of utilization has been identified.

## Alert Severity

### Warning

A warning indicates a developing condition requiring investigation but not necessarily immediate service interruption.

### Critical

A critical alert indicates reduced operational margin or a condition with greater potential impact and should receive prompt investigation.

## Automated Remediation

Automated restart or remediation is not part of the initial monitoring baseline.

Automation should only be introduced for well-understood failure conditions where:

- the trigger is reliable,
- the corrective action is predictable,
- unintended impact is understood,
- evidence requirements are addressed,
- successful recovery can be independently verified.

## Escalation

If the condition cannot be safely identified or corrected:

- preserve available evidence,
- avoid unnecessary configuration changes,
- document the observed state,
- perform additional investigation before destructive remediation.
