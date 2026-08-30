# Command Reference — RED-003

All commands below were used only against the authorized isolated lab target `192.168.50.20`.

## Validate KALI01
```bash
ip addr show eth0
ip route
```

## Neighbor Cache
```bash
ip neigh
```

## Basic TCP Scan
```bash
nmap 192.168.50.20
```

## Service / Version Detection
```bash
nmap -sV 192.168.50.20
```

## Full TCP Port Scan
```bash
nmap -p- 192.168.50.20
```

## Targeted Service Detection
```bash
nmap -sV -p 3632,6697,8787,41818,42311,46989,50476 192.168.50.20
```

A nearby high-port value was mistyped during one console entry, and some high-numbered listeners changed state between scans. Results were therefore treated as time-specific observations.

## Limited UDP Scan
```bash
sudo nmap -sU -p 53,69,111,137,161,2049 192.168.50.20
```

Observed:
- `53/udp` — open
- `69/udp` — open|filtered
- `111/udp` — open
- `137/udp` — closed
- `161/udp` — closed
- `2049/udp` — open

## Syntax Learned
- `-sV` — service/version detection
- `-sU` — UDP scan
- `-p` — specify port(s)
- `-p-` — scan all TCP ports
- comma-separated values after `-p` — scan multiple specific ports
