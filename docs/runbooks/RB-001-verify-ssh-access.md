# RB-001 — Verify and Recover SSH Access

## Purpose

Verify that SSH public-key authentication is operational and identify recovery steps if remote access fails.

## Normal Connection

From Windows PowerShell:

```powershell
ssh ubuntu-lab

Expected behavior:

The client requests the private-key passphrase.
The session opens as randy.
The Ubuntu account password is not requested.
Confirm Identity

On Ubuntu:

whoami
hostname

Expected values:

randy
ubuntu
Review Effective SSH Security Settings
sudo sshd -T | grep -E \
'pubkeyauthentication|passwordauthentication|kbdinteractiveauthentication|permitrootlogin'

Expected output:

pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
permitrootlogin no
Validate Configuration Syntax
sudo sshd -t

Success produces no output.

Do not reload SSH if an error is returned.

Reload SSH Safely
sudo systemctl reload ssh

Reload rereads configuration without intentionally terminating existing sessions.

Test Password Rejection

From Windows PowerShell:

ssh -o PubkeyAuthentication=no `
    -o PreferredAuthentications=password,keyboard-interactive `
    ubuntu-lab

Expected result:

Permission denied (publickey).
Recovery Procedure

If normal key authentication fails:

Do not close any existing working SSH session.
Open the Ubuntu VM console through Proxmox.
Log in locally with the randy account password.
Validate SSH configuration:
sudo sshd -t
Confirm the public key exists:
cat ~/.ssh/authorized_keys
Confirm permissions and ownership:
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys

Expected permissions:

drwx------ ~/.ssh
-rw------- ~/.ssh/authorized_keys
Review effective settings:
sudo sshd -T | grep -E \
'pubkeyauthentication|passwordauthentication|kbdinteractiveauthentication|permitrootlogin'
Inspect configuration snippets:
sudo ls -l /etc/ssh/sshd_config.d
sudo grep -RniE \
'PubkeyAuthentication|PasswordAuthentication|KbdInteractiveAuthentication|PermitRootLogin' \
/etc/ssh/sshd_config /etc/ssh/sshd_config.d
Correct the problem, validate with sshd -t, then reload SSH.
Emergency Rollback

From the Proxmox console, temporarily rename the hardening file:

sudo mv \
  /etc/ssh/sshd_config.d/01-homelab-hardening.conf \
  /etc/ssh/sshd_config.d/01-homelab-hardening.conf.disabled

Validate and reload:

sudo sshd -t
sudo systemctl reload ssh

This may restore settings from other SSH configuration files. Review effective settings before relying on password access.


## Commit the documentation

After saving all three files:

```bash
git status

Review the actual content being added:

git diff --check
git diff --stat

git diff --check looks for whitespace errors. git diff --stat summarizes changed files.

Stage them:

git add docs/

Review the staged snapshot:

git diff --staged --stat

Commit:

git commit -m "Document SSH authentication and recovery"

Verify:

git log --oneline --decorate -2
git status

You should have two commits and a clean working tree. This is now real, persistent project documentation—not just an idea buried in our conversation.