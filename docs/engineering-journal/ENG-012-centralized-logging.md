# ENG-012 — Centralized Logging

## Purpose

LAB-011 was designed to answer a question that monitoring alone cannot:

What happened before or during an event?

LAB-010 provided visibility into infrastructure health and alerting. LAB-011 adds retained historical evidence that can be searched after an event occurs.

## Design Approach

The logging architecture uses three primary components:

### Grafana Alloy

Alloy collects logs from the Ubuntu host and Docker environment.

It performs collection and metadata processing before forwarding events to Loki.

### Grafana Loki

Loki provides centralized log storage and querying.

It uses filesystem-backed storage located on a dedicated logical volume.

### Grafana

Grafana provides the investigation interface used to search and explore Loki data.

This extends the existing observability platform rather than introducing a separate interface solely for logs.

## Host Logs vs Container Logs

An important distinction established during the lab was the difference between host and container evidence.

The Ubuntu systemd journal primarily answers:

"What happened to the server?"

Examples include operating system, service, and SSH activity.

Docker logs primarily answer:

"What happened inside the services running on the server?"

These sources overlap operationally but provide different investigative perspectives.

Both are valuable when reconstructing an event.

## Authentication Evidence

SSH was prioritized because remote administrative access represents a high-value security event.

The systemd journal provides evidence associated with SSH activity, allowing the logging platform to establish when remote access activity occurred and provide additional authentication context.

Journal metadata was enriched so SSH activity could be queried directly using the `ssh.service` systemd unit instead of relying only on text searches.

## Storage Decision

Unlimited log retention was intentionally rejected.

Retention requirements depend on factors including:

- service importance
- log volume
- available storage
- number of users
- operational requirements
- security risk
- investigation requirements

For the current environment, a 6 GB dedicated logical volume and 30-day retention period were selected as an initial baseline.

The decision can be revisited after actual log-growth data exists.

## Why Dedicated Storage

Allowing centralized logging to grow freely on the root filesystem creates unnecessary operational risk.

A dedicated logical volume:

- separates log storage from the operating system filesystem
- creates a defined capacity boundary
- makes utilization easier to monitor
- reduces the possibility that unexpected log growth consumes root filesystem capacity

Approximately 3 GB remains unallocated in the volume group, preserving flexibility rather than allocating all available capacity immediately.

## Ingestion Validation

Initial testing proved that Alloy could send logs to Loki and that Loki could return them through queries.

Further testing demonstrated an important distinction:

A running collector does not prove reliable logging.

Successful logging requires validation of:

- ingestion
- timestamps
- continuity
- metadata
- searchability

The usefulness of monitoring or security analysis depends on the quality of the underlying data.

Bad data produces bad conclusions.

## Docker Replay Finding

During Alloy recreation, historical Docker entries were replayed.

Loki rejected entries that were outside its acceptable ingestion window, and one older entry preceded the configured schema start date.

Persistent Alloy state was therefore added through a Docker volume and explicit storage path.

This allows collection state, including Docker read positions, to survive container recreation.

After implementation, a later recreation generated one rejected stale entry from historical Loki container output. No continuing ingestion errors occurred after startup.

The result demonstrated why logging systems must be validated at the data layer rather than judged only by container health.

## Security Design

Neither Loki nor Alloy is published directly to the host LAN.

This follows the principle that an internal backend should not automatically become a network-accessible service simply because it exposes an HTTP interface.

Only components that require communication with the logging backend are connected to the necessary Docker networks.

Alloy requires read-only Docker socket access for the current container discovery design.

The Docker socket remains a privileged interface and should continue to be treated as security-sensitive.

## Scope Management

LAB-011 intentionally stopped at centralized logging.

Detection engineering was not included.

Collection answers:

"What happened?"

Detection begins asking:

"Which events should be considered suspicious or important?"

Detection introduces additional concepts including thresholds, event classification, false positives, alert quality, correlation, and response.

Separating these capabilities prevents scope creep and ensures future detection work is built on validated data.

Or, less formally:

The goal was for LAB-011 to take days, not three weeks.

## Engineering Lessons

### Monitoring and Logging Are Different

Metrics describe system state and trends.

Logs preserve event-level evidence.

Both are necessary for effective observability and investigation.

### Host and Application Evidence Complement Each Other

The system journal provides host context while Docker logs provide service and application context.

An investigation may require both.

### Storage Is Part of Logging Architecture

Retention cannot be designed independently of storage capacity.

A retention policy without capacity planning is only an assumption.

### Collector Health Is Not Data Health

A running Alloy container does not guarantee that events are successfully reaching Loki.

The ingestion path must be tested.

### Persistent State Matters

Collectors may maintain positions or offsets that determine where collection resumes.

Container recreation without persistent state can affect log continuity or cause historical data to be replayed.

### Data Quality Determines Investigation Quality

A logging platform is only useful if its underlying evidence is trustworthy.

Bad or incomplete data can produce bad operational or security conclusions.

### Scope Boundaries Improve Engineering

Centralized logging, security detection, and incident response are related but distinct capabilities.

Stopping at a deliberate boundary allows each capability to be implemented and validated independently.

## Outcome

LAB-011 established the centralized logging foundation for Yerigan Labs.

Ubuntu journal events, SSH activity, and Docker container logs can now be retained and searched through Loki and Grafana.

This creates the historical evidence layer required for future detection engineering, security investigation, and incident response work.
