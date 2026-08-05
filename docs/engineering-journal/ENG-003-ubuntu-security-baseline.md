# ENG-003 — Ubuntu Security Baseline

**Status:** In Progress  
**Date:** August 4, 2026

## Objective

Reduce the Ubuntu Docker host's attack surface and establish repeatable security controls without disrupting required services.

## Changes Implemented

### Portainer

Portainer was migrated from a manually created standalone container into Docker Compose.

The existing `portainer_data` volume was preserved.

The unused Edge Agent publication was removed.

Current host publication:

```text
192.168.1.203:9443 -> container TCP 9443