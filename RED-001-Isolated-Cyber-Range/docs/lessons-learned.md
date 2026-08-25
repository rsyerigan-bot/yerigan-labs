# RED-001 Lessons Learned

## Isolation and addressing are different concepts
A host can be connected to an isolated Layer-2 network and still have no IPv4 address. Isolation explains why the RED network does not have an external path; the absence of a DHCP server explains why TARGET01 did not automatically receive an IPv4 configuration.

## Same-subnet traffic does not need a router
KALI01 (`192.168.50.10/24`) and TARGET01 (`192.168.50.20/24`) are in the same subnet. Kali determines that the destination is local, uses ARP to resolve the target's MAC address, and sends frames directly over `vmbr1`. A default gateway is unnecessary for this communication.

## Configuration present is not configuration active
The `vmbr1` troubleshooting episode demonstrated that a bridge visible as a pending Proxmox configuration is not necessarily instantiated in the running network stack. The VM error was a useful indicator to check whether network changes had actually been applied.

## Old guest operating systems may need old virtual hardware
Metasploitable 2 predates many modern virtualization defaults. SeaBIOS/i440fx/E1000/IDE provide a compatibility-oriented baseline and avoid introducing unnecessary driver problems into a lab whose purpose is security training rather than guest-driver troubleshooting.

## Verify artifacts before trusting them
SHA-256 validation established that the downloaded/transferred installation artifacts were unchanged before deployment. This is a repeatable supply-chain/integrity habit, not merely an installation step.

## Validate controls instead of assuming them
The lab was not considered isolated merely because `vmbr1` was named an isolated bridge. Validation included inspecting interface addressing, checking the routing table for the absence of a default route, confirming the target's bridge assignment, and testing only expected local communication.

## Troubleshooting is part of the lab outcome
The most useful skills exercised were not only installation steps. Boot failures, pending network changes, virtual-disk conversion/import, console-input problems, and interpreting Linux network state all required hypothesis → test → correction cycles.
