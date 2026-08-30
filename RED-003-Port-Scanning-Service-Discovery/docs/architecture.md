# Architecture — RED-003

## Environment

```text
Proxmox VE
|
+-- vmbr1 — isolated RED network (192.168.50.0/24)
    |
    +-- KALI01 (VM 101)
    |   +-- eth0: 192.168.50.10/24
    |   +-- Role: authorized assessment workstation
    |
    +-- TARGET01 (VM 102)
        +-- eth0: 192.168.50.20/24
        +-- Role: intentionally vulnerable Metasploitable 2 target
```

`vmbr1` has no intended default gateway, DNS service, or Internet route.

## Pre-Scan Validation
KALI01 showed `192.168.50.10/24` on `eth0`, a directly connected route for `192.168.50.0/24`, and no `default via` route.

## Restart Troubleshooting
The first basic scan reported TARGET01 as down and `ip neigh` was empty. Both VMs had been shut down since RED-002.

TARGET01's address had previously been assigned at runtime with `ifconfig`, so it did not survive reboot. After restoring `192.168.50.20/24`, the original Nmap command succeeded.

### Lesson
When known-good behavior changes after a restart, validate system state before changing scanner options or assuming filtering.
