# Security Hardening Guide

This guide covers best practices for securing your VPS, including firewall setup, SSH hardening, and other security measures.

## Table of Contents

1. [Firewall Configuration](#firewall-configuration)
2. [SSH Hardening](#ssh-hardening)
3. [Disable Root Login](#disable-root-login)
4. [Implement Fail2Ban](#implement-fail2ban)
5. [Keep Software Updated](#keep-software-updated)
6. [Secure Shared Memory](#secure-shared-memory)
7. [Set Up Automatic Security Updates](#set-up-automatic-security-updates)

## Firewall Configuration

We'll use UFW (Uncomplicated Firewall) for this guide:

1. Install UFW:
   ```bash
   sudo apt install ufw
   ```

2. Set default policies:
   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   ```

3. Allow SSH (adjust if you use a different port):
   ```bash
   sudo ufw allow 22/tcp
   ```

4. Allow HTTP and HTTPS:
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

5. Enable the firewall:
   ```bash
   sudo ufw enable
   ```

## SSH Hardening

Edit the SSH configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Make the following changes:

1. Change the default SSH port:
   ```
   Port 2222
   ```

2. Disable password authentication:
   ```
   PasswordAuthentication no
   ```

3. Limit user logins:
   ```
   AllowUsers your_username
   ```

4. Disable empty passwords:
   ```
   PermitEmptyPasswords no
   ```

5. Use strong ciphers:
   ```
   Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
   ```

After making changes, restart the SSH service:

```bash
sudo systemctl restart sshd
```

## Disable Root Login

In `/etc/ssh/sshd_config`, set:

```
PermitRootLogin no
```

## Implement Fail2Ban

1. Install Fail2Ban:
   ```bash
   sudo apt install fail2ban
   ```

2. Create a local configuration file:
   ```bash
   sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
   ```

3. Edit the local configuration:
   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```

4. Configure the SSH jail:
   ```
   [sshd]
   enabled = true
   port = 2222
   filter = sshd
   logpath = /var/log/auth.log
   maxretry = 3
   bantime = 3600
   ```

5. Restart Fail2Ban:
   ```bash
   sudo systemctl restart fail2ban
   ```

## Keep Software Updated

Regularly update your system:

```bash
sudo apt update
sudo apt upgrade
```

## Secure Shared Memory

Edit `/etc/fstab` and add:

```
tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0
```

## Set Up Automatic Security Updates

1. Install unattended-upgrades:
   ```bash
   sudo apt install unattended-upgrades
   ```

2. Configure it:
   ```bash
   sudo dpkg-reconfigure -plow unattended-upgrades
   ```

These steps will significantly improve the security of your VPS. Remember to regularly review and update your security measures as new threats and best practices emerge.