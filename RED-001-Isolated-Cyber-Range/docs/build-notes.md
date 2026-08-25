# RED-001 Build Notes

## 1. Proxmox baseline
The Proxmox host reported approximately 15.5 GB RAM, 431 GB VM storage, and 8 logical CPUs. An existing Ubuntu Server VM was already present.

## 2. KALI01
Kali installation media was downloaded and its SHA-256 digest was checked before installation. KALI01 was created with 2 vCPU, 4 GB RAM, and a 60 GB disk. During installation, the initial UEFI/OVMF path did not boot the installer correctly; changing the VM boot approach allowed installation to proceed.

After installation, the QEMU Guest Agent was installed/started. Browser VNC exhibited intermittent lag and repeated keystrokes, so SSH was used where practical for reliable command entry.

## 3. RED bridge
A Linux bridge named `vmbr1` was created for the isolated network. The first VM start attempt failed with:

```text
TASK ERROR: bridge 'vmbr1' does not exist
```

The configuration existed as a pending change but was not active. Applying the Proxmox network configuration activated the bridge. `vmbr1` has no physical bridge port.

KALI01 was then placed on the RED network and configured as `192.168.50.10/24` with no default gateway.

## 4. TARGET01 / Metasploitable 2
The Metasploitable archive contained VMware files including `Metasploitable.vmdk`. The VMDK size was approximately 1.93 GB. SHA-256 verification was performed before and after transfer/import preparation.

TARGET01 (VM 102) was created with 1 vCPU and 1 GB RAM. Its E1000 NIC was attached to `vmbr1`.

The VMDK was imported with Proxmox's `qm importdisk`. This produced an unused imported disk in TARGET01. The small placeholder disk created by the VM wizard was detached/removed. The imported disk was attached as a legacy-compatible IDE boot disk.

An initial boot-order check showed `net0, ide2`, which would not boot the imported system disk. Boot order was corrected so the imported disk was the active boot device.

TARGET01 then successfully booted Metasploitable 2.

## 5. Static target addressing
TARGET01 initially showed only loopback IPv4 plus a MAC/link-local IPv6 address on `eth0`. This was expected because the isolated bridge did not contain a DHCP server.

TARGET01 was assigned:

```text
192.168.50.20/24
```

No gateway or DNS server was configured.

## 6. Validation
KALI01 retained `192.168.50.10/24` after reboot. Its route table contained the directly connected `192.168.50.0/24` route and no default route.

KALI01 successfully pinged `192.168.50.20`. Its neighbor table then contained TARGET01, demonstrating successful ARP resolution across `vmbr1`.

## Representative commands

```bash
# Proxmox VM inspection
qm config 102

# Import existing VMware disk
qm importdisk 102 /tmp/Metasploitable.vmdk local-lvm

# TARGET01 static address (legacy Metasploitable userspace)
sudo ifconfig eth0 192.168.50.20 netmask 255.255.255.0 up

# Kali validation
ip addr
ip route
ip neigh
ping -c 3 192.168.50.20
ip neigh
```
