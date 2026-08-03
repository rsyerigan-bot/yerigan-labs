# ENG-001 — SSH Key Authentication and Hardening

**Status:** Complete  
**Completed:** August 3, 2026

## Objective

Replace password-based remote SSH authentication with passphrase-protected Ed25519 key authentication while preserving a safe recovery path during implementation.

## Environment

- Client: Windows workstation
- Server: Ubuntu Server VM
- Server address: `192.168.1.203`
- Server account: `randy`
- Hypervisor: Proxmox VE
- Emergency recovery path: Proxmox VM console

## Background

SSH provides encrypted remote administration of the Ubuntu server.

The server initially accepted the Ubuntu account password as a remote authentication method. This worked, but exposed a reusable credential to password-guessing and brute-force attempts.

Public-key authentication separates the credentials into:

- A private key retained on the Windows client
- A public key stored on the Ubuntu server
- A passphrase that encrypts and unlocks the private key locally

The private key is never sent to the server.

## Implementation

### Generate the key pair on Windows

```powershell
ssh-keygen -t ed25519 -C "Randall Homelab"

Command breakdown:

ssh-keygen generates SSH keys.
-t ed25519 selects the Ed25519 algorithm.
-C adds a human-readable comment.
A passphrase was added to protect the private key.

Files created:

C:\Users\rsyer\.ssh\id_ed25519
C:\Users\rsyer\.ssh\id_ed25519.pub
Install the public key on Ubuntu

The public key was placed in:

/home/randy/.ssh/authorized_keys

Permissions were restricted:

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
700 allows only the owner to access the .ssh directory.
600 allows only the owner to read or modify authorized_keys.
Create a Windows SSH profile

The Windows SSH client configuration was updated:

Host ubuntu-lab
    HostName 192.168.1.203
    User randy
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

This permits the shortened connection command:

ssh ubuntu-lab
Harden the Ubuntu SSH server

A dedicated configuration snippet was created:

/etc/ssh/sshd_config.d/01-homelab-hardening.conf

Configuration:

PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
Troubleshooting

The hardening file was initially named:

99-homelab-hardening.conf

The effective configuration still reported:

passwordauthentication yes

Investigation showed that:

/etc/ssh/sshd_config.d/50-cloud-init.conf

was processed first and defined PasswordAuthentication yes.

OpenSSH uses the first obtained value for many configuration keywords. Renaming the administrator-controlled file to:

01-homelab-hardening.conf

caused it to load before the cloud-init file.

This demonstrated that valid syntax and correct file contents do not guarantee the intended effective configuration. Configuration precedence must also be verified.

Validation

Syntax validation:

sudo sshd -t

Effective configuration review:

sudo sshd -T | grep -E \
'pubkeyauthentication|passwordauthentication|kbdinteractiveauthentication|permitrootlogin'

Expected values:

pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
permitrootlogin no

SSH was reloaded without terminating existing sessions:

sudo systemctl reload ssh

Key-based authentication succeeded through:

Windows PowerShell
VS Code Remote SSH

A forced password-only connection failed as intended.

Recovery Controls

During implementation:

Existing SSH sessions remained open.
Password authentication was not disabled until key login was verified.
The Proxmox console remained available for emergency access.
SSH configuration was validated before reload.
Concepts Learned
SSH client versus SSH server configuration
Public and private key roles
Private-key passphrases
Linux ownership and permissions
Modular configuration files
Configuration precedence
Effective configuration validation
Safe service reload procedures
Evidence-based troubleshooting
Engineering Principle

Trust effective configuration and test results, not assumptions based only on edited files.

Career Relevance

These practices apply to:

Linux systems administration
Cloud virtual machines
Secure server management
DevOps and infrastructure automation
Privileged-access management
Security hardening
Incident prevention
Reflection

SSH access now depends on possession of the private key and knowledge of its passphrase. The Ubuntu account password remains available for local console access and sudo, but it is no longer accepted as a remote SSH login method.