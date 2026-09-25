# RB-004 — Troubleshoot and Recover Caddy Reverse-Proxy Access

## Purpose

Troubleshoot and recover access to Yerigan Labs services that are reached through the Caddy reverse proxy.

The goal is to identify the failed layer before changing configuration or restarting services.

## Scope

This runbook applies to the current internal reverse-proxy architecture:

    Client
      |
      v
    Friendly DNS name
      |
      v
    192.168.4.203:80
      |
      v
    Caddy
      |
      v
    Docker proxy network or host service
      |
      v
    Backend application

## Current Architecture

Caddy is the primary internal HTTP entry point and is published on:

    192.168.4.203:80

Current documented routes include:

    home.yerigan.home.arpa       -> homepage:3000
    status.yerigan.home.arpa     -> uptime-kuma:3001
    dns.yerigan.home.arpa        -> pihole:80
    portainer.yerigan.home.arpa  -> portainer:9000
    ha.yerigan.home.arpa         -> 192.168.4.203:8123
    grafana.yerigan.home.arpa    -> grafana:3000

The internal `http://caddy` site also proxies Home Assistant webhook traffic to the host on TCP/8123.

Most containerized web services communicate with Caddy through the external Docker network named `proxy`.

## Safety Requirements

Before changing reverse-proxy configuration:

1. Identify whether one service or all proxied services are affected.
2. Preserve the current Caddyfile before making changes.
3. Inspect logs before restarting containers when practical.
4. Validate configuration before reloading Caddy.
5. Change one layer at a time.
6. Do not publish a backend container port merely to bypass Caddy.
7. Do not weaken UFW as a first troubleshooting step.

## Troubleshooting Model

Investigate in this order:

    DNS
      |
      v
    Host TCP/80
      |
      v
    Caddy container
      |
      v
    Caddy configuration
      |
      v
    Docker proxy network
      |
      v
    Backend service
      |
      v
    Application behavior

This order helps isolate the failed layer instead of treating every proxy failure as a Caddy problem.

## Step 1 — Determine the Failure Scope

Test more than one friendly service URL.

If every proxied service fails, prioritize shared dependencies such as:

- DNS
- host connectivity
- TCP/80
- Caddy
- the shared proxy network

If only one service fails, prioritize:

- that Caddy route
- that container
- that container's proxy-network membership
- the backend application

Record which services work and which fail before changing anything.

## Step 2 — Verify DNS Resolution

From a client, resolve the affected friendly name.

On Windows:

    nslookup home.yerigan.home.arpa

Use the actual affected hostname when troubleshooting another service.

The expected current destination is:

    192.168.4.203

If the hostname does not resolve correctly, investigate DNS before modifying Caddy.

A DNS failure is not evidence that the reverse proxy itself is broken.

## Step 3 — Verify Host Reachability and TCP/80

From Windows PowerShell:

    Test-NetConnection 192.168.4.203 -Port 80

If TCP/80 cannot be reached:

1. confirm the Ubuntu host is reachable
2. inspect Caddy container status
3. inspect the host listener
4. inspect Docker publication
5. inspect UFW

On Ubuntu:

    docker ps --filter name=caddy
    sudo ss -lntp
    docker ps --format 'table {{.Names}}\t{{.Ports}}'
    sudo ufw status numbered

The expected Caddy publication is bound to the Ubuntu LAN IPv4 address on TCP/80.

## Step 4 — Verify Caddy Container State

Check the container:

    docker ps --filter name=caddy

If it is not running, inspect its state before restarting it:

    docker ps -a --filter name=caddy
    docker logs --tail 100 caddy

Look for configuration errors, startup failures, networking errors, or repeated restarts.

Do not assume a stopped container should simply be restarted without first reviewing available evidence.

## Step 5 — Validate the Caddy Configuration

The repository source of truth is:

    infrastructure/docker/caddy/Caddyfile

From the Caddy infrastructure directory, validate the active configuration syntax using the running container when available:

    docker exec caddy caddy validate --config /etc/caddy/Caddyfile

A successful validation should report that the configuration is valid.

If validation fails:

1. do not reload Caddy
2. identify the reported configuration error
3. compare the repository configuration with the intended route
4. correct only the identified problem
5. validate again

## Step 6 — Inspect Caddy Logs

Review recent Caddy logs:

    docker logs --tail 100 caddy

For ongoing observation during a controlled test:

    docker logs -f caddy

Stop the live log view with Ctrl+C after capturing the relevant event.

Look for evidence such as:

- upstream connection failures
- name-resolution failures
- malformed configuration
- backend connection refusal
- container startup problems

Absence of an obvious Caddy error does not prove the backend application is healthy.

## Step 7 — Verify the Docker Proxy Network

Confirm that the shared network exists:

    docker network ls

Inspect it:

    docker network inspect proxy

Verify that Caddy and the affected container are attached when the route depends on Docker service-name resolution.

For routes such as:

    homepage:3000
    uptime-kuma:3001
    pihole:80
    portainer:9000
    grafana:3000

Caddy must be able to resolve and reach the backend through an appropriate shared Docker network.

Do not assign static container IP addresses merely to work around a Docker DNS or network-membership problem.

## Step 8 — Verify the Backend Container

Check the affected container:

    docker ps

If necessary, inspect its recent logs:

    docker logs --tail 100 CONTAINER_NAME

Replace `CONTAINER_NAME` with the actual container name.

Determine whether:

- the container is running
- the application started successfully
- the expected internal port is correct
- the container is attached to the expected network
- the application is accepting connections

A healthy Caddy container cannot compensate for an unavailable backend.

## Step 9 — Verify Network Membership From the Docker Host

For routes that use Docker service-name resolution, verify that Caddy and the affected backend share the `proxy` network.

Inspect Caddy's network attachments:

    docker inspect -f '{{json .NetworkSettings.Networks}}' caddy

Inspect the affected backend:

    docker inspect -f '{{json .NetworkSettings.Networks}}' CONTAINER_NAME

Both containers must show membership in the `proxy` network for Caddy to reach that backend by Docker service name.

This check confirms Docker network attachment. It does **not** by itself prove DNS resolution or application-layer reachability.

If network membership is correct, continue with backend state, logs, and application-level checks rather than assuming Docker DNS is functioning correctly.

## Step 10 — Special Case: Home Assistant

Home Assistant currently differs from most proxied services.

The documented route is:

    ha.yerigan.home.arpa -> 192.168.4.203:8123

Caddy also proxies internal webhook requests to:

    192.168.4.203:8123

Home Assistant therefore depends on host-level TCP/8123 access in the current architecture rather than only Docker service-name resolution.

When troubleshooting Home Assistant proxy access, verify:

    sudo ss -lntp
    sudo ufw status numbered

and confirm Home Assistant itself is running before changing Caddy.

Direct LAN TCP/8123 access is currently accepted temporary exposure and remains scheduled for later review.

## Step 11 — Reload Caddy Safely

Reload only after configuration validation succeeds.

Use:

    docker exec caddy caddy reload --config /etc/caddy/Caddyfile

Then:

1. review Caddy logs
2. retest the affected friendly URL
3. test at least one previously working proxied service
4. confirm the change did not create a regression

A container restart should not be the default method for applying a valid Caddy configuration change.

## Recovery When Caddy Configuration Is Broken

If a Caddy configuration change causes proxy failure:

1. preserve the failed configuration for investigation
2. compare it with the last known-good Git version
3. identify the specific change responsible
4. restore or correct only the affected configuration
5. validate the Caddyfile
6. reload Caddy
7. retest the affected service
8. test another proxied service
9. review logs for remaining errors

Useful Git inspection commands include:

    git diff -- infrastructure/docker/caddy/Caddyfile
    git log --oneline -- infrastructure/docker/caddy/Caddyfile

Do not use `git reset --hard` as a routine recovery method because unrelated working-tree changes may exist.

## Failure Pattern Guide

### Friendly hostname does not resolve

Likely layer:

    DNS

Investigate DNS before Caddy.

### Host resolves but TCP/80 fails

Likely layers:

    Caddy container
    Docker publication
    host listener
    UFW

### All friendly URLs return proxy errors

Likely layers:

    Caddy
    shared Docker proxy network
    common infrastructure dependency

### One friendly URL fails while others work

Likely layers:

    individual Caddy route
    backend container
    backend network membership
    backend application

### Caddy resolves backend but connection is refused

Likely layers:

    backend application
    incorrect backend port
    service not listening

### Direct backend works but friendly URL fails

Likely layers:

    DNS
    Caddy route
    proxy-network path

Do not permanently expose the backend merely because direct access helps isolate the problem.

## Success Criteria

Recovery is complete when:

- the friendly hostname resolves as intended
- TCP/80 is reachable from the expected LAN
- Caddy is running
- the Caddy configuration validates
- the affected backend is healthy
- required Docker proxy-network membership is correct
- the friendly URL works
- at least one other proxied service still works
- no unnecessary host publication or firewall exception was introduced

## Evidence to Capture

For significant failures or configuration changes, preserve relevant evidence such as:

- DNS lookup result
- TCP/80 connectivity test
- Caddy container state
- Caddy validation result
- relevant Caddy logs
- Docker proxy-network inspection
- backend container state and logs
- Caddyfile diff
- final functional test
- regression-test result
