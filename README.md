
# 🐧 Linux System Administration Portfolio: Zero to Root
**Author:** Saleem Ali
**Focus:** SysAdmin, Security, Networking, & DevOps
**Tech Stack:** RHEL/Ubuntu, Bash, Systemd, SELinux, Networking, LVM

---

## 📋 Comprehensive Lab Index
*Click any module to jump to the code.*

| Module | Labs Covered | Key Concepts |
| :--- | :--- | :--- |
| **1. Filesystem Core** | 1–4, 8, 9, 36, 42 | Nav, `cp/mv`, Links (Soft/Hard), Mounting, `find` |
| **2. Users & Permissions** | 5–6, 25–27, 60 | `chmod/chown`, User Mgmt, Password Policies, SELinux |
| **3. Text & Editors** | 32–35, 51 | `vim` mastery, `sed`, `awk`, `grep` & Regex |
| **4. Process & Packages** | 14–15, 21–22, 38, 59 | `systemd`, `ps/top`, `kill`, `apt/yum`, `htop` |
| **5. Storage & Archives** | 18–20, 41, 43–45, 54 | LVM, Partitions, Swap, `tar/gzip`, Disk Usage |
| **6. Networking & SSH** | 17, 30–31, 39–40, 52 | SSH Keys, SCP, `ip addr`, UFW, IPTables, `curl` |
| **7. Shell Environment** | 10–13, 16, 46–50, 58 | `.bashrc`, Aliases, History, Redirection, Pipes |
| **8. Logs & Monitoring** | 23, 37, 53, 56–57 | `journalctl`, `/var/log`, Log Rotation, Hardware Info |

---

## 🟢 Module 1: Filesystem Core & Navigation

### 📂 Lab 1-4 & 36: Navigation & Search
* **Goal:** Move fast without a mouse.
```bash
# Lab 1: Where am I?
pwd
ls -la # Show hidden files

# Lab 36: The Power Search
# Find all .log files larger than 10MB modified in the last 7 days
find /var/log -name "*.log" -size +10M -mtime -7

```

### 🔗 Lab 9: Links (Hard vs Soft)

* **Goal:** Create shortcuts and references.

```bash
# Soft Link (Shortcut) - breaks if original moves
ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/

# Hard Link - points to same inode (data survives delete)
ln original_file hard_link

```

### 💾 Lab 42 & 45: Mounting & Fstab

* **Goal:** Attach drives permanently.

```bash
# Manual Mount
mount /dev/sdb1 /mnt/data

# Lab 45: Permanent Mount (/etc/fstab)
# UUID=1234-5678 /mnt/data ext4 defaults 0 2

```

---

## 🔵 Module 2: Users, Permissions & Security

### 🔐 Lab 5, 6 & 26: Permissions (chmod/chown)

* **Goal:** Secure files.

```bash
# Lab 5: Numeric Permissions
chmod 755 script.sh  # rwx for owner, rx for others
chmod 600 key.pem    # rw for owner, NO access for anyone else (Secure)

# Lab 6: Ownership
chown user:group file.txt

```

### 👤 Lab 25 & 27: User Management

* **Goal:** Create users and enforce passwords.

```bash
# Create user with home dir (-m) and shell (-s)
useradd -m -s /bin/bash newuser
passwd newuser

# Lab 27: Password Aging (Force change every 90 days)
chage -M 90 newuser

```

### 🛡️ Lab 60: SELinux (The Security Layer)

* **Goal:** Mandatory Access Control.

```bash
# Check status
sestatus

# Temporarily set to Permissive (Log only, don't block)
setenforce 0

# Permanent change: /etc/selinux/config -> SELINUX=enforcing

```

---

## 🟠 Module 3: Text Processing & Editors

### 📝 Lab 32: Vim Mastery

* **Goal:** Edit files on servers without a GUI.
* **Cheatsheet:**
* `i` -> Insert Mode.
* `Esc` -> Command Mode.
* `:wq` -> Save & Quit.
* `dd` -> Delete line.
* `u` -> Undo.



### 🔍 Lab 33, 34, 35: The "Holy Trinity" (Grep, Sed, Awk)

* **Goal:** Manipulate data streams.

```bash
# Lab 33: Grep (Search)
grep -r "Error" /var/log/syslog

# Lab 34: Sed (Replace)
# Replace 'http' with 'https' in config file
sed -i 's/http/https/g' config.conf

# Lab 35: Awk (Columns)
# Print only User and CPU usage from process list
ps aux | awk '{print $1, $3}'

```

---

## 🟣 Module 4: Process & Service Management

### ⚙️ Lab 38: Systemd (Services)

* **Goal:** Manage background daemons.

```bash
# Enable service to start at boot
systemctl enable nginx

# Check logs for a specific service
journalctl -u nginx -f

```

### 🚦 Lab 14 & 59: Monitoring (Top/Htop)

* **Goal:** Find resource hogs.

```bash
# Classic View
top

# Interactive View (Install if needed)
htop

# Kill a stuck process
kill -9 <PID>
pkill -f python  # Kill all python processes

```

---

## 🟤 Module 5: Storage & Partitioning

### 💿 Lab 41 & 44: Partitioning & LVM

* **Goal:** Manage disks dynamically.

```bash
# Lab 41: List Block Devices
lsblk

# Lab 44: LVM Workflow
pvcreate /dev/sdb       # 1. Initialize Disk
vgcreate data_vg /dev/sdb # 2. Create Group
lvcreate -L 10G -n data_lv data_vg # 3. Create Volume

```

### 📦 Lab 18 & 19: Archives (Tar/Gzip)

* **Goal:** Backup data.

```bash
# Create compressed backup
tar -czvf backup.tar.gz /var/www/html

# Extract
tar -xzvf backup.tar.gz

```

---

## 🔴 Module 6: Networking & SSH

### 🌐 Lab 17: Network Config (Modern)

* **Goal:** Check IPs and DNS.

```bash
# Old: ifconfig
# Modern Standard:
ip addr show
ip route show

# DNS Lookup
dig google.com

```

### 🔑 Lab 30: SSH Keys (Passwordless Login)

* **Goal:** Secure remote access.

```bash
# 1. Generate Key Pair
ssh-keygen -t rsa -b 4096

# 2. Send Public Key to Server
ssh-copy-id user@remote-server

# 3. Login (No password needed)
ssh user@remote-server

```

### 🔥 Lab 39 & 40: Firewalls (UFW/IPTables)

* **Goal:** Block ports.

```bash
# UFW (Ubuntu)
ufw allow 22/tcp  # Allow SSH
ufw enable

# IPTables (RHEL/Legacy)
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

```

---

## ⚪ Module 7: Logs, Monitoring & Environment

### 📜 Lab 37 & 56: Analyzing Logs

* **Goal:** Troubleshooting.

```bash
# Watch logs in real-time
tail -f /var/log/syslog

# Filter Cron logs
grep "CRON" /var/log/syslog

```

### 🐚 Lab 46 & 58: Aliases & .bashrc

* **Goal:** Shortcuts for productivity.

```bash
# Add to ~/.bashrc
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade -y'

# Apply changes
source ~/.bashrc

```

---

**Status:** Completed
**Certification Level:** Equivalent to RHCSA / LFCS
