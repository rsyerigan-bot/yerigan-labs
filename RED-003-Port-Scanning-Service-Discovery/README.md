# RED-003 — Port Scanning & Service Discovery

## Status
Practical assessment: **PASS**
Knowledge check: **PASS**
Lab closure: Pending Git commit/push

## Objective
Continue from RED-002 by taking one authorized discovered host and determining which TCP/UDP ports are exposed and what services appear to be listening.

## Scope
- Authorized target: `192.168.50.20` (TARGET01 / Metasploitable 2)
- Attacker: `192.168.50.10` (KALI01)
- Network: `192.168.50.0/24` on isolated Proxmox bridge `vmbr1`
- No home-LAN systems, Proxmox management interfaces, Internet systems, or other hosts were authorized targets.

## Stopping Point
RED-003 covered port scanning and service identification only. It did **not** include vulnerability validation or exploitation.

## Lab Progression
1. Validated Kali addressing and routing.
2. Troubleshot an initially unavailable target after VM shutdown/restart.
3. Performed a default Nmap TCP scan.
4. Performed service/version detection with `-sV`.
5. Scanned the full TCP port range with `-p-`.
6. Performed targeted version detection against newly discovered ports.
7. Performed a limited UDP scan.
8. Interpreted `open`, `closed`, `filtered`, and `open|filtered`.
9. Completed knowledge check and scope review.

## Key Results
The default TCP scan tested Nmap's normal set of 1,000 common TCP ports and found 23 open ports. A full `-p-` scan found 30 open TCP ports at that moment, demonstrating that a default scan is not equivalent to full TCP-port discovery.

Service/version detection identified examples including OpenSSH, Apache httpd, MySQL, PostgreSQL, VNC, UnrealIRCd, Apache Tomcat/Coyote, distccd, Ruby DRb, and Java RMI.

A limited UDP scan of ports `53,69,111,137,161,2049` produced a mixture of `open`, `closed`, and `open|filtered` states.

## Core Lessons
- An open port proves a listening endpoint was observed; a conventional port label does not by itself prove the actual application.
- `-sV` probes services to identify application/protocol and often version information.
- Default scanning can miss services on less-common ports.
- Scan results are observations at a point in time; dynamic/transient listeners can change state.
- UDP scanning is more ambiguous because silence does not necessarily mean a port is closed.
- Reachability never expands authorization.
- Discovery and identification come before vulnerability research; exploitation is a separate controlled phase.
