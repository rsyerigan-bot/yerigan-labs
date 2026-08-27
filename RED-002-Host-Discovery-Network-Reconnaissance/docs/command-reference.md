# RED-002 Command Reference

```bash
ip addr
```
Inspect interfaces and assigned addresses.

```bash
ip route
```
Inspect the routing table and verify whether a default route exists.

```bash
nmap -sn 192.168.50.0/24
```
Perform host discovery without the normal port-scan phase.

```bash
ip neigh
```
Inspect learned local neighbor entries.

```bash
sudo arp-scan --interface=eth0 192.168.50.0/24
```
Actively discover responding IPv4 hosts on the local Ethernet segment using ARP.

All active discovery was limited to the explicitly authorized isolated lab subnet.
