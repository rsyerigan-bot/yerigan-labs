# RED-002 — Host Discovery & Network Reconnaissance

## Objective
Identify live hosts on an authorized isolated IPv4 subnet without being given the target IP address, and compare local host-discovery methods.

## Scope
**Authorized:** isolated Proxmox RED network `192.168.50.0/24`, containing KALI01 and TARGET01.

**Out of scope:** home LAN (`192.168.1.0/24`), Proxmox management, Ubuntu server, router, Internet systems, and any system not explicitly placed in the RED range.

## Environment
- KALI01: Kali Linux attack workstation
- TARGET01: Metasploitable 2
- Proxmox bridge: `vmbr1`
- RED subnet: `192.168.50.0/24`
- No default gateway
- No Internet route

## Learning Objectives
- Establish local network context before scanning.
- Derive the local subnet from interface configuration.
- Distinguish a neighbor cache from active discovery.
- Perform host discovery without a port scan.
- Use ARP-based discovery on a local IPv4 segment.
- Explain the roles of IP addresses, MAC addresses, and ARP.
- Maintain authorization and scope discipline.

## Procedure

### 1. Situational Awareness
```bash
ip addr
ip route
```

Observed:
- IPv4: `192.168.50.10/24`
- Network: `192.168.50.0/24`
- Broadcast: `192.168.50.255`
- Default route: none
- Internet route: none

### 2. Nmap Host Discovery
```bash
nmap -sn 192.168.50.0/24
```

Two hosts were reported up:
- `192.168.50.10` — KALI01
- `192.168.50.20` — another live host

`-sn` was intentionally used for host discovery rather than port scanning.

### 3. Neighbor Cache
```bash
ip neigh
```

The neighbor table is not a complete subnet inventory. It represents neighbors Kali has learned about through local communication.

### 4. ARP-Based Discovery
```bash
sudo arp-scan --interface=eth0 192.168.50.0/24
```

One remote host responded, with both an IPv4 address and MAC address.

## Results

| Method | Purpose | Result |
|---|---|---|
| `ip addr` | Identify own interface/address | `192.168.50.10/24` |
| `ip route` | Identify reachable networks | RED subnet only; no default route |
| `nmap -sn` | Active host discovery | Two hosts up |
| `ip neigh` | Inspect learned neighbors | Learned IP-to-MAC mappings |
| `arp-scan` | Active local ARP discovery | One remote host plus MAC |

## Key Lessons
- `ip neigh` answers what local neighbors the host has already learned; active discovery deliberately searches for hosts.
- ARP maps a local IPv4 address to a Layer-2 MAC address.
- IP addresses are logical Layer-3 addresses and can be private or public.
- MAC addresses identify network interfaces at Layer 2 and can be changed or spoofed.
- A host failing to respond does not prove an address is unused.
- Technical reachability does not create authorization.

## Scope Response
If discovery unexpectedly reveals an out-of-scope system:
1. Stop scanning.
2. Verify KALI01 interfaces and routes.
3. Verify Proxmox bridge configuration.
4. Correct any isolation problem.
5. Do not probe the unexpected system.

## Outcome
**PASS**

Workflow demonstrated:

`Situational Awareness → Subnet Identification → Active Discovery → ARP Discovery → Interpretation → Scope Validation`

No exploitation, vulnerability scanning, or port/service enumeration was performed.

## Next Lab
RED-003 continues from **“What hosts exist?”** to **“What network services does the discovered host expose?”**
