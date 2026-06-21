
# Linux Hardening & Recovery Protocol

## Executive Summary
This repository documents a structured approach to incident response and production-level server hardening. It covers the full lifecycle from emergency recovery of a compromised system to the implementation of a hardened security baseline.

## 1. Incident Response & Diagnostics
*Immediate steps taken to identify and neutralize threats.*

### Process & File Analysis
```bash
# Identify suspicious resource consumption
top -c
# Inspect active network connections
ss -tulpn
# Find recently modified files (last 24h)
find / -mtime -1 -ls

### Log Investigation
```bash
# Analyze SSH brute-force attempts
grep "Failed password" /var/log/auth.log | tail -n 20
# Examine Nginx error logs for exploit patterns
tail -f /var/log/nginx/error.log
```

## 2. Remediation
*Neutralizing unauthorized access and cleaning system artifacts.*
```bash
# Terminate malicious processes
kill -9 <PID>
# Purge unauthorized artifacts
rm -rf /tmp/.hidden_dir

# Credential Audit
nano ~/.ssh/authorized_keys # Remove unauthorized keys
passwd <username>           # Reset compromised account
```

## 3. Hardening Baseline (The "Fortress" Setup)
*Proactive measures to minimize the attack surface.*
### SSH Security
### Edit /etc/ssh/sshd_config

```bash
Port 2222
PermitRootLogin no
PasswordAuthentication no

### Network Perimeter (UFW)

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow 2222/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw enable

### Filesystem & Nginx Hardening
# Prevent execution in /tmp (Add to /etc/fstab)

```bash
tmpfs /tmp tmpfs defaults,noexec,nosuid 0 0
mount -o remount /tmp

# Nginx hardening (nginx.conf)
server_tokens off;
```

## 4. Proactive Monitoring (Fail2Ban)
```bash
apt install fail2ban -y

# Configured /etc/fail2ban/jail.local to auto-ban brute-force actors
systemctl restart fail2ban




