# RB-011 — Centralized Log Investigation

## Purpose

Provide a repeatable process for using centralized logs during troubleshooting or security investigation.

## Initial Response

When investigating an event:

1. Establish the approximate event time.
2. Avoid unnecessary rebooting or remediation before evidence is reviewed.
3. Determine whether host, authentication, container, or multiple evidence sources are relevant.
4. Search the narrowest useful time window first.
5. Expand the investigation window as necessary.

## Ubuntu Host Logs

In Grafana Explore using the Loki data source, query:

    {job="systemd-journal"}

Use host logs to investigate operating system and service activity.

The systemd journal primarily helps answer:

"What happened to the server?"

## SSH Activity

Query SSH service events:

    {job="systemd-journal", unit="ssh.service"}

Use SSH evidence to investigate remote authentication and administrative access activity.

Authentication evidence has high security value because it can help establish when remote access activity occurred and provide context about access to the server.

## Docker Logs

Query all collected Docker logs:

    {job="docker"}

Query a specific container:

    {job="docker", container="<container-name>"}

Search a specific container for a term:

    {job="docker", container="<container-name>"} |= "error"

Docker logs primarily help answer:

"What happened inside the services running on the server?"

## Correlation

When investigating an event, compare evidence from multiple sources using the same approximate time window.

A basic investigation may include:

1. Identify unusual host or service behavior.
2. Review SSH activity around the same time.
3. Determine which containers were active or affected.
4. Review relevant container logs.
5. Compare log findings with infrastructure metrics available in Grafana.

Events occurring at approximately the same time may be related, but temporal correlation alone does not prove causation.

## Logging Platform Health

Check the logging containers:

    cd ~/yerigan-labs/infrastructure/docker/logging
    docker compose ps

Check recent Alloy ingestion errors:

    docker logs alloy --since 10m 2>&1 \
      | grep -Ei 'error|dropping|too far behind|no schema' \
      || echo "PASS: no recent Alloy ingestion errors"

Check Loki storage:

    df -hT /var/lib/loki
    sudo du -sh /var/lib/loki

## Known Behavior

During testing, Alloy replayed a stale historical Docker log entry following container recreation.

Loki rejected the historical entry because its timestamp was outside the acceptable ingestion window.

After the initial startup rejection, current ingestion continued without additional errors.

An isolated historical rejection followed by clean current ingestion does not necessarily indicate an ongoing collection failure.

Repeated or continuing dropped-data messages should be investigated.

## Evidence Preservation

Do not immediately reboot a system solely because a problem exists.

A reboot may alter or destroy useful evidence and does not establish root cause.

When operational conditions allow, review available evidence before remediation.

Useful evidence may include:

- centralized logs
- infrastructure metrics
- active user sessions
- running processes
- container state
- authentication activity
- relevant network activity

Preserve evidence appropriate to the event before making changes that may alter system state.

## Storage Check

Loki data is stored on the dedicated filesystem mounted at:

    /var/lib/loki

Check capacity with:

    df -hT /var/lib/loki

Current retention is configured for 30 days.

Storage consumption should be monitored over time because retention requirements and storage requirements are directly related.

## Escalation

Escalate the investigation when:

- current logs are repeatedly being dropped
- Loki becomes unavailable
- the logging filesystem approaches capacity
- an expected log source disappears
- authentication activity appears unauthorized
- timestamps or event continuity appear unreliable
- evidence suggests a security incident rather than a routine operational problem

## Scope

This runbook covers centralized log investigation and logging-platform health.

Security detection rules, automated response, SIEM correlation, and formal incident-response procedures are outside the scope of LAB-011 and should be developed as separate capabilities.
