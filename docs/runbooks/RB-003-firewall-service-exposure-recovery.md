# RB-003 — Verify and Recover Host Firewall and Service Exposure

## Purpose

Verify the effective network exposure of the Yerigan Labs Ubuntu host and troubleshoot service-access problems without weakening security controls as a first response.

## Scope

This runbook applies to the Ubuntu Docker host and its current UFW, Docker port-publishing, and LAN service-exposure controls.

It is intended for situations where:

- an expected service cannot be reached
- a service appears reachable when it should not be
- a firewall or Docker networking change has been made
- the effective exposure of a service needs to be verified
- access must be recovered after a configuration change

## Current Security Model

The intended host firewall baseline is:

- incoming traffic: deny by default
- outgoing traffic: allow by default
- routed traffic: deny by default
- required LAN services explicitly permitted
- Docker host publications minimized
- web applications exposed through Caddy where practical

Do not assume that UFW alone describes Docker service exposure. Docker port publishing and host listeners must also be inspected.

## Safety Requirements

Before changing firewall or network configuration:

1. Preserve any working SSH session.
2. Confirm Proxmox console access is available when practical.
3. Record the current state before making changes.
4. Change one control at a time.
5. Do not disable UFW as a first troubleshooting action.
6. Do not expose a container port merely to test whether the application works.
7. Validate the effective state after every change.

## Step 1 — Verify Host Addressing

Confirm the Ubuntu host interfaces and addresses:

    ip addr

Confirm routing:

    ip route

Expected LAN address for the current environment:

    192.168.1.203

If the host address has changed, investigate addressing before modifying firewall rules.

## Step 2 — Verify UFW State

Display the active firewall policy and numbered rules:

    sudo ufw status verbose
    sudo ufw status numbered

Expected baseline:

    Status: active
    Default: deny (incoming), allow (outgoing), deny (routed)

Review the actual rules rather than assuming previously documented rules are still active.

## Step 3 — Inspect Listening Sockets

Inspect TCP and UDP listeners:

    sudo ss -lntup

Pay attention to:

- listening address
- protocol
- port
- owning process

A listener on 127.0.0.1 is local-only.

A listener on 192.168.1.203 is bound to the LAN IPv4 address.

A listener on 0.0.0.0 or [::] is broadly bound and requires additional firewall and exposure review.

## Step 4 — Inspect Docker Port Publishing

Review running containers:

    docker ps

Review published ports in a concise format:

    docker ps --format 'table {{.Names}}\t{{.Ports}}'

Current design expects most web applications to avoid direct host publication and instead use the shared Docker proxy network.

Examples currently documented as having no direct host web publication include:

- Portainer
- Uptime Kuma
- Homepage

Pi-hole DNS and Caddy intentionally require host publication for their current roles.

## Step 5 — Compare Listener, Docker, and Firewall Evidence

For the service being investigated, determine all three:

1. Is anything listening on the host?
2. Is Docker publishing the service?
3. Does UFW permit the traffic?

Do not infer exposure from only one evidence source.

For Docker workloads, remember that Docker networking can affect traffic independently of ordinary host-firewall assumptions.

## Step 6 — Test From the Appropriate Network Location

Test expected LAN access from another trusted LAN device.

Examples from Windows PowerShell:

    Test-NetConnection 192.168.1.203 -Port 22
    Test-NetConnection 192.168.1.203 -Port 80
    Test-NetConnection 192.168.1.203 -Port 8123

Only test ports that are relevant to the service being investigated.

A successful LAN test demonstrates LAN reachability. It does not prove Internet reachability.

## Step 7 — Investigate an Unexpectedly Unreachable Service

If an expected service is unreachable, check in this order:

1. Confirm the Ubuntu host is reachable.
2. Confirm the expected host address.
3. Confirm the application or container is running.
4. Confirm the expected listener exists.
5. Confirm Docker publication or proxy-network connectivity as applicable.
6. Confirm UFW contains the intended rule.
7. Test from another trusted LAN system.
8. Review application, container, proxy, and firewall logs as appropriate.

Do not immediately add a broad allow rule.

## Step 8 — Investigate Unexpected Exposure

If a service appears reachable when it should not be:

1. Identify the exact address and port reached.
2. Inspect host listeners with `ss`.
3. Inspect Docker publications.
4. Review UFW rules.
5. Determine whether the listener is IPv4, IPv6, or both.
6. Identify whether Caddy or another proxy intentionally provides access.
7. Remove or restrict the actual source of exposure only after it is identified.
8. Retest from the same network location.

## Recovery After a Firewall Change

If a firewall change disrupts access:

1. Keep any existing administrative session open.
2. Use the Proxmox console if remote access is lost.
3. Review the current rules:

       sudo ufw status numbered

4. Identify the specific rule or policy responsible.
5. Correct only the affected rule.
6. Recheck firewall status.
7. Test access from a second system.
8. Confirm the intended security baseline is restored.

Avoid using:

    sudo ufw disable

as a routine troubleshooting step.

Disabling the firewall removes an important control and can obscure the actual cause of the problem.

## Current Known Exposure Notes

The current security baseline documents:

- SSH TCP/22 for trusted-LAN administration
- Pi-hole TCP/UDP 53 for LAN DNS
- Caddy TCP/80 as the internal reverse-proxy entry point
- Home Assistant TCP/8123 as accepted temporary direct-LAN exposure
- go2rtc TCP/18554 bound locally
- go2rtc TCP/18555 observed listening broadly without an explicit inbound UFW allow rule
- no direct host publication for Portainer, Uptime Kuma, or Homepage

These statements describe the documented baseline and must still be verified against the live system during troubleshooting.

## Known Limitations

The current baseline does not establish that the environment is unreachable from the Internet.

The following remain separate verification work:

- router port-forward review
- UPnP review
- IPv6 firewall review
- DMZ configuration review
- remote-administration review
- independent external reachability testing

Do not report those controls as validated until that work is actually performed.

## Success Criteria

The investigation is complete when:

- the relevant listener state is known
- Docker publication is understood
- applicable UFW rules are understood
- expected LAN reachability has been tested
- unexpected exposure or loss of access has an identified cause
- any corrective change has been independently retested
- the firewall remains enabled
- unresolved external-exposure questions remain explicitly documented

## Evidence to Capture

When this runbook is used for a significant incident or configuration change, preserve relevant evidence such as:

- `ip addr`
- `ip route`
- `sudo ufw status verbose`
- `sudo ufw status numbered`
- `sudo ss -lntup`
- `docker ps`
- remote connectivity-test results
- relevant logs
- configuration diff
- final validation result
