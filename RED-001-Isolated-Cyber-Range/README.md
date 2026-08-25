# RED-001 — Isolated Cyber Range & Vulnerable Target Deployment

## Objective
Build a controlled virtual cyber range in Proxmox for authorized offensive-security training. Deploy Kali Linux as the attacker workstation and Metasploitable 2 as an intentionally vulnerable target while preventing the RED network from reaching the home LAN or Internet.

## Scope
This lab covers virtualization, network isolation, artifact validation, VM deployment, static addressing, routing validation, ARP, and ICMP. Exploitation is intentionally out of scope; that begins in later RED labs.

## Environment
- Proxmox VE 9.2.5 on an HP ProDesk 600 G4 SFF
- Intel Core i7-7700, 16 GB DDR4, ~431 GB usable VM storage
- KALI01 (VM 101): Kali Linux, 2 vCPU, 4 GB RAM, 60 GB disk
- TARGET01 (VM 102): Metasploitable 2, 1 vCPU, 1 GB RAM
- `vmbr0`: normal/home network bridge
- `vmbr1`: isolated RED bridge with no physical bridge port

## Final Architecture

```text
                    HOME LAN / INTERNET
                            |
                          vmbr0
                            |
                    Proxmox Host
                            |
                   [isolation boundary]
                            |
                          vmbr1
                   192.168.50.0/24
                       /        \
                      /          \
                 KALI01        TARGET01
              192.168.50.10   192.168.50.20
                 attacker     Metasploitable 2

                  No default gateway
                       No DNS
                  No Internet route
```

## Security Design
The vulnerable target is attached only to `vmbr1`. The bridge has no physical uplink, and the RED hosts have no default gateway. This allows Layer-2/Layer-3 communication between lab systems without intentionally routing the target onto the household network or Internet.

## Build Summary
1. Verified the Kali installation ISO with SHA-256 before use.
2. Created KALI01 and installed Kali Linux.
3. Installed and started the QEMU Guest Agent.
4. Created and activated `vmbr1` as the isolated RED bridge.
5. Moved KALI01 onto `vmbr1` and configured `192.168.50.10/24` with no gateway.
6. Downloaded/extracted Metasploitable 2 and verified the VMDK with SHA-256.
7. Created TARGET01, attached its E1000 NIC to `vmbr1`, and imported the VMware VMDK into Proxmox.
8. Removed the temporary placeholder disk and attached the imported disk as the boot disk using legacy-compatible virtual hardware.
9. Booted TARGET01 and configured `192.168.50.20/24` with no gateway or DNS.
10. Verified KALI01 had only a directly connected `192.168.50.0/24` route and no default route.
11. Verified ARP neighbor discovery and successful ICMP connectivity from KALI01 to TARGET01.

## Validation Results
| Control / Test | Result |
|---|---|
| Artifact integrity | SHA-256 matched for downloaded/imported media |
| TARGET01 network attachment | `vmbr1` only |
| RED bridge external uplink | None |
| KALI01 address | `192.168.50.10/24` |
| TARGET01 address | `192.168.50.20/24` |
| Default gateway | None on isolated RED configuration |
| Direct RED route | `192.168.50.0/24` via Kali RED interface |
| ARP / neighbor discovery | TARGET01 learned by KALI01 |
| ICMP | Successful KALI01 → TARGET01 |
| Internet path from RED network | No default route configured |

## Key Networking Outcome
An isolated Layer-2 network does not automatically assign IP addresses. TARGET01 initially had a MAC address but no IPv4 address because `vmbr1` contained no DHCP server. After assigning static addresses in the same `/24`, Kali and TARGET01 could communicate directly without a router. Kali used ARP to resolve TARGET01's IPv4 address to its MAC address and then exchanged ICMP traffic across the virtual bridge.

## Troubleshooting Highlights
The build included several realistic failures: an interrupted ISO upload, a Kali UEFI/OVMF boot-path issue, QEMU Guest Agent behavior, noVNC keyboard lag/double presses, an inactive `vmbr1` producing `bridge 'vmbr1' does not exist`, and an incorrect TARGET01 boot order after importing the VMDK. These are documented in `docs/build-notes.md` and `docs/lessons-learned.md`.

## Evidence
Selected screenshots are stored under `evidence/`. They intentionally avoid credentials and secrets.

## Outcome
**RED-001 COMPLETE:** An isolated, reusable two-host cyber range is operational and validated. The environment is ready for a separate follow-on lab focused on authorized network discovery and service enumeration.
