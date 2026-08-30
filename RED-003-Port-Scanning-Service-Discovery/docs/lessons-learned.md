# Lessons Learned — RED-003

## Port Number Is Not Service Identity
A result such as `22/tcp open ssh` establishes that TCP/22 was observed open. Without service/version detection, the service name should not be treated as proof of the actual application.

## Version Detection Adds Context
`-sV` produced more specific fingerprints such as OpenSSH, Apache httpd, MySQL, Apache Tomcat/Coyote, distccd, Ruby DRb, and Java RMI. Product/version information supports later vulnerability research, but a version string alone does not prove vulnerability.

## Default Scan Is Not a Full Port Scan
The default scan found 23 open TCP ports among its normal 1,000-port selection. The full TCP scan observed 30 open TCP ports at that time.

## Scan Results Are Time-Specific Evidence
Some high-numbered ports were observed open during one scan and closed during later validation. A defensible report records both observations and avoids unsupported explanations.

## TCP vs. UDP
TCP is connection-oriented and provides ordered/reliable byte-stream delivery mechanisms. UDP is connectionless and sends independent datagrams without TCP's built-in delivery and ordering guarantees. UDP silence is therefore often ambiguous during scanning.

## Filtered Does Not Mean Modified Data
`filtered` means Nmap cannot determine the port's state because a firewall, filter, or other network obstacle may prevent sufficient probe/response information from reaching the scanner.

`open|filtered` means Nmap cannot distinguish between those two states from the evidence received.

## Conventional Labels Are Not Confirmation
`69/udp open|filtered tftp` does not prove TFTP is running. UDP/69 is conventionally associated with TFTP while the state remains ambiguous.

## Scope Overrides Curiosity
If another host appears while only `192.168.50.20` is authorized, do not probe it. Stop, verify scope/environment, and correct or report the anomaly.

## Assessment Progression
```text
Host discovery
    ↓
Port discovery
    ↓
Service/version identification
    ↓
Vulnerability research and validation
    ↓
Controlled exploitation (only when authorized)
```

RED-003 stops before vulnerability research/exploitation.
