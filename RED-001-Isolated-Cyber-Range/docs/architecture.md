# RED-001 Architecture

## Logical topology

```text
Home router / DHCP / Internet
          |
        vmbr0
          |
     Proxmox host
          |
        vmbr1  <-- internal RED bridge; no physical port
       /     \
      /       \
 KALI01      TARGET01
 .50.10       .50.20
 /24          /24
```

## Address plan
| System | Role | Address | Gateway | DNS |
|---|---|---|---|---|
| KALI01 | Attacker workstation | 192.168.50.10/24 | None | None required |
| TARGET01 | Vulnerable training target | 192.168.50.20/24 | None | None |

## Isolation rationale
`vmbr1` functions as a virtual Layer-2 segment. Because it has no physical bridge port and the lab hosts have no default route, the design keeps normal RED traffic local to the Proxmox host. Same-subnet hosts do not require a router: they use ARP to learn each other's MAC addresses and exchange Ethernet frames directly.

## Virtual hardware choices
TARGET01 is an old operating system image, so legacy-compatible settings were favored: SeaBIOS, i440fx, E1000 networking, and an IDE-attached imported boot disk. KALI01 uses modern virtualized hardware suitable for Kali.
