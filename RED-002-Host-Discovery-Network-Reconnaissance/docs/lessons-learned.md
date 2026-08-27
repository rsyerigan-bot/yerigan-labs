# RED-002 Lessons Learned

1. **Know yourself before scanning.** Interface and routing information establish both operator identity and the authorized network boundary.
2. **A neighbor table is not an inventory.** `ip neigh` contains neighbors learned through communication; silent hosts may not appear.
3. **Host discovery and port scanning are separate tasks.** `nmap -sn` deliberately stopped before port enumeration.
4. **ARP is powerful locally.** It discovers local IPv4 neighbors and associates IP addresses with MAC addresses, but ARP does not cross routers.
5. **IP and MAC addresses serve different layers.** IP provides Layer-3 logical addressing; MAC provides Layer-2 interface addressing for local Ethernet delivery.
6. **No response does not prove no host exists.** Filtering, reachability, configuration, and probe selection affect discovery.
7. **Scope overrides curiosity.** Unexpected out-of-scope systems trigger lab-network troubleshooting, not investigation of the unknown system.
