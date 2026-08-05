# Yerigan Labs Target Architecture

**Status:** Draft  
**Planning Horizon:** 2–3 years

## Mission

Yerigan Labs is designed to serve three connected purposes:

1. Provide reliable and secure digital services for the family.
2. Demonstrate professional infrastructure and cybersecurity capabilities.
3. Develop reusable standards, procedures, and technical patterns for a future MSP or MSSP.

## Design Goals

- Secure by design
- Clearly segmented
- Easy to understand and maintain
- Recoverable from hardware or configuration failure
- Documented and version controlled
- Useful to the household
- Suitable for portfolio demonstrations
- Capable of supporting future client simulations and service development

## Architectural Principles

- Design before deployment
- Default deny where practical
- Least privilege
- Separate management, infrastructure, family, IoT, guest, and lab traffic
- Prefer private service communication over exposed ports
- Use Git for configuration and documentation
- Make changes incrementally
- Validate every change
- Maintain a rollback path
- Automate repeatable work

## Target Security Zones

### Management Zone

Purpose:

- Administrative access to infrastructure

Systems:

- Proxmox
- Portainer
- Network management
- SSH
- Monitoring dashboards
- Backup administration

Access:

- Restricted to approved administrative workstations
- Future VPN access permitted
- No general family or IoT access

### Infrastructure Zone

Purpose:

- Core services required by the environment

Systems:

- DNS
- Reverse proxy
- Monitoring
- Logging
- Authentication
- Certificate services
- Backup services

Access:

- Limited by service requirement
- Management access only from the Management Zone

### Enterprise Lab Zone

Purpose:

- Career development and client-environment simulation

Systems:

- Windows Server
- Active Directory
- Windows clients
- Linux systems
- Vulnerability scanners
- SIEM
- Test applications

Access:

- Isolated from production family systems
- Controlled Internet access
- Managed access from the Management Zone

### Family Services Zone

Purpose:

- Reliable household services

Systems:

- Jellyfin
- Immich
- Document storage
- Family dashboard
- Shared applications

Access:

- Available to trusted family devices
- No direct administrative access
- Limited communication with storage and infrastructure services

### IoT Zone

Purpose:

- Smart-home and consumer IoT devices

Systems:

- Lights
- Thermostats
- Speakers
- Appliances
- Sensors
- Home Assistant integrations

Access:

- Internet access only when required
- No direct access to management or enterprise systems
- Limited access to Home Assistant, DNS, NTP, and required controllers

### Camera Zone

Purpose:

- Cameras, doorbells, and video-security equipment

Access:

- Prefer no direct Internet access
- Communication limited to the NVR or approved management systems

### Guest Zone

Purpose:

- Visitor devices

Access:

- Internet only
- No access to internal systems

## Target Platform Layout

```text
Internet
    |
Edge Router / Firewall
    |
    +-- Management Zone
    |     +-- Admin workstation
    |     +-- Proxmox
    |     +-- Portainer
    |
    +-- Infrastructure Zone
    |     +-- DNS
    |     +-- Reverse proxy
    |     +-- Monitoring
    |     +-- Logging
    |     +-- Backups
    |
    +-- Enterprise Lab Zone
    |     +-- Windows Server
    |     +-- Active Directory
    |     +-- Windows client
    |     +-- Security tools
    |
    +-- Family Services Zone
    |     +-- Jellyfin
    |     +-- Immich
    |     +-- Shared storage
    |
    +-- IoT Zone
    |     +-- Home Assistant
    |     +-- Smart-home devices
    |
    +-- Camera Zone
    |     +-- Cameras
    |     +-- NVR
    |
    +-- Guest Zone
          +-- Internet-only clients