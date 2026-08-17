# ENG-011 — Observability and Monitoring Foundation

## Purpose

Document the implementation, validation, troubleshooting, and operational findings associated with the initial Yerigan Labs infrastructure observability platform.

## Initial Problem

Existing monitoring primarily answered whether services were available.

This did not provide sufficient visibility into the health of the underlying Ubuntu host or Docker workloads.

A service could remain reachable while resource exhaustion, performance degradation, or another developing condition existed underneath it.

## Design Approach

The monitoring architecture was intentionally separated by responsibility.

### Node Exporter

Collects host-level telemetry from the Ubuntu platform.

### cAdvisor

Collects Docker container telemetry.

### Prometheus

Scrapes and stores time-series metrics.

Initial validated targets included:

- `node-exporter:9100`
- `cadvisor:8080`

Both targets reported healthy scrape status.

### Grafana

Provides visualization and alert evaluation.

Grafana communicates with Prometheus across the internal monitoring network and is the only monitoring component intended to provide a user-facing graphical interface.

### Caddy

Provides controlled access to Grafana and an internal routing path used for selected alert delivery.

### Home Assistant

Provides mobile notification orchestration for higher-value infrastructure alerts.

## Network Design

Prometheus, Node Exporter, and cAdvisor remain on the dedicated monitoring Docker network.

Grafana participates in both the monitoring network and the existing proxy network.

This allows Grafana to query monitoring data while also being reachable through Caddy without publishing Grafana directly on the Ubuntu host.

## Grafana Access

Grafana is accessed through:

`grafana.yerigan.home.arpa`

Caddy reverse proxies requests to the Grafana container.

During implementation, the repository Caddy configuration was found to differ from the configuration mounted by the running Caddy container.

The running container referenced:

`/home/randy/docker/caddy/Caddyfile`

while the repository configuration existed under:

`~/yerigan-labs/infrastructure/docker/caddy/`

This configuration drift initially prevented the new Grafana route from becoming active.

The discrepancy was identified by inspecting the running container mounts and Docker Compose labels.

## DNS Troubleshooting

The Grafana hostname was configured in Pi-hole and resolved correctly when Pi-hole was queried directly.

The Windows workstation continued returning NXDOMAIN because IPv6 DNS configuration caused queries to use Google DNS rather than Pi-hole.

Testing the Pi-hole server directly confirmed that the local DNS record itself was correct.

This demonstrated the importance of identifying the actual resolver used by the client rather than assuming the configured IPv4 resolver controlled all DNS traffic.

## Initial Dashboard

The first host-health dashboard included:

- CPU utilization
- Memory utilization
- Root filesystem utilization
- Network throughput
- Host uptime

Network throughput was limited to the Ubuntu host's `ens18` interface to avoid confusing physical-interface traffic with Docker bridge and virtual-interface telemetry.

## Major Operational Finding

The root filesystem dashboard immediately reported approximately 99% utilization.

This was independently validated with operating-system tools.

Observed state:

- Filesystem: `/dev/mapper/ubuntu--vg-ubuntu--lv`
- Filesystem type: ext4
- Size: approximately 19 GB
- Used: approximately 18 GB
- Available: approximately 230 MB
- Utilization: 99%
- Inode utilization: approximately 34%

The condition therefore represented storage-capacity exhaustion rather than inode exhaustion.

## Investigation

Top-level disk usage showed substantial utilization under `/var`, including Docker-related storage.

Docker inspection showed approximately 7 GB of images but almost no reclaimable image capacity.

This indicated that indiscriminate Docker cleanup would not meaningfully address the root problem.

Block-device and LVM inspection showed:

- Virtual disk: 40 GB
- LVM physical volume: approximately 38 GB
- Root logical volume: approximately 19 GB
- Free capacity in volume group: approximately 19 GB

The underlying storage already existed but had not been allocated to the root logical volume.

## Remediation

Rather than deleting legitimate data or introducing external storage, the root logical volume was expanded by 10 GB.

Approximately 9 GB was intentionally retained as free volume-group capacity to preserve future flexibility.

Following expansion, root filesystem utilization decreased to approximately 66%.

The monitoring platform itself provided confirmation that the remediation had changed the observed condition.

## NAS Consideration

External NAS storage was considered during investigation.

It was rejected as a solution to this specific condition because sufficient unused local storage already existed.

Introducing network-attached storage to solve an LVM allocation problem would have added unnecessary network and availability dependencies without addressing the actual root cause.

NAS storage remains potentially appropriate for future backups, archives, media, or other data requirements.

## Alert Design

Static thresholds were considered appropriate for CPU, memory, and filesystem capacity when combined with persistence periods.

Network throughput requires baseline and anomaly-oriented analysis rather than a simple fixed threshold.

Uptime is more useful as a state-change indicator because an unexpected reset may indicate a reboot or instability.

The initial production alert implementation was intentionally limited to root filesystem utilization.

### Warning

Greater than 85% for 10 minutes.

### Critical

Greater than 90% for 5 minutes.

The shorter critical persistence period reflects lower risk tolerance as available filesystem capacity decreases.

## Alert Validation

The warning threshold was temporarily reduced to create a safe controlled test condition without intentionally consuming disk capacity.

Grafana successfully transitioned the alert through:

Normal → Pending → Firing

The production threshold was restored following validation.

## Notification Architecture

Grafana was configured to send selected alerts to a Home Assistant webhook.

Direct Grafana-to-Home-Assistant communication on port 8123 timed out because the monitoring network did not have the same firewall access previously granted to the proxy path.

Rather than widening direct Home Assistant access, alert traffic was routed through Caddy.

The resulting path is:

Grafana → Caddy → Home Assistant → Mobile Device

An initial internal hostname failed because Docker DNS could not resolve the custom hostname.

The configuration was corrected to use the Docker-resolvable `caddy` service name.

A separate webhook delivery failure was traced to an incorrect webhook identifier containing an unintended leading hyphen.

After correction, Grafana contact-point testing successfully generated a Home Assistant mobile notification.

The real root-filesystem warning rule was then associated with the contact point and successfully generated a mobile notification when intentionally placed into the firing state.

## Engineering Lessons

### Availability Is Not Health

All major services could remain operational while the root filesystem was approximately 99% utilized.

Availability monitoring alone would not have identified the developing failure condition.

### Evidence Should Be Validated

Grafana's 99% reading was independently verified using host-level tools before remediation was attempted.

### Investigate Before Cleaning

Deleting Docker data would have treated a symptom without identifying the actual storage architecture.

LVM inspection showed that sufficient local capacity already existed.

### Separation of Concerns Is Not Redundancy

The monitoring components perform distinct functions.

Exporters collect telemetry, Prometheus stores it, Grafana interprets and visualizes it, Caddy controls routing, and Home Assistant handles selected notification delivery.

### Monitoring and Remediation Are Different Controls

An alert should not automatically trigger a reboot simply because restarting may temporarily restore normal behavior.

Unexpected restart actions may destroy diagnostic or security evidence and can conceal the actual root cause.

The initial policy therefore favors detection, notification, evidence preservation, investigation, deliberate remediation, and validation.

## Outcome

Yerigan Labs now has a centralized observability foundation capable of collecting historical host and container telemetry, presenting host-health information, evaluating defined alert conditions, and delivering selected infrastructure alerts to the administrator's mobile device.

The implementation also identified and enabled remediation of a real storage-capacity risk before a service outage occurred.

Further monitoring capabilities will be implemented as separate scoped engineering work rather than expanding LAB-010 indefinitely.
