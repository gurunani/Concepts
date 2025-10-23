# Linux Filesystem Hierarchy (FHS) - Quick Reference

## Visual Directory Tree

```
/  (root - the starting point of everything)
│
├── bin/          → Essential user commands (ls, cp, mv, cat)
├── sbin/         → System admin commands (reboot, fdisk, mount)
├── lib/          → Essential shared libraries for /bin and /sbin
├── lib64/        → 64-bit libraries (on 64-bit systems)
│
├── boot/         → Boot loader files (kernel, initrd, grub)
├── dev/          → Device files (hard drives, terminals, USB)
├── etc/          → System configuration files (passwd, fstab, ssh)
│
├── home/         → User home directories (/home/username)
├── root/         → Root user's home directory
│
├── tmp/          → Temporary files (cleared on reboot)
├── var/          → Variable data (logs, mail, databases, caches)
│   ├── log/      → System and application log files
│   ├── cache/    → Application cache data
│   ├── lib/      → Variable state information
│   └── spool/    → Print queues, mail queues
│
├── usr/          → User programs and data (secondary hierarchy)
│   ├── bin/      → User commands
│   ├── sbin/     → Non-essential system commands
│   ├── lib/      → Libraries for /usr/bin and /usr/sbin
│   ├── local/    → Local software installations
│   ├── share/    → Architecture-independent data
│   └── src/      → Source code
│
├── opt/          → Optional/third-party software packages
│
├── proc/         → Virtual filesystem (running processes, system info)
├── sys/          → Virtual filesystem (kernel and hardware info)
│
├── mnt/          → Temporary mount points (manual mounting)
├── media/        → Mount points for removable media (USB, CD/DVD)
│
├── run/          → Runtime data (PID files, sockets) - tmpfs
└── srv/          → Data for services (web server files, FTP)
```

## Quick Category Summary

### 🔧 **System Binaries & Libraries**
- `/bin/` - Essential commands everyone needs
- `/sbin/` - System administration commands
- `/lib/`, `/lib64/` - Essential libraries
- `/usr/bin/`, `/usr/sbin/`, `/usr/lib/` - Additional programs & libraries

### ⚙️ **Configuration & Boot**
- `/etc/` - All configuration files
- `/boot/` - Kernel and boot loader files

### 🏠 **User Data**
- `/home/` - Regular user directories
- `/root/` - Administrator's home

### 📝 **Variable & Temporary Data**
- `/var/` - Logs, databases, caches (grows over time)
- `/tmp/` - Temporary files (auto-cleaned)
- `/run/` - Runtime data since boot

### 💾 **Hardware & Virtual**
- `/dev/` - Device files (hardware interfaces)
- `/proc/` - Live system information
- `/sys/` - Kernel and hardware control

### 📦 **Software & Services**
- `/usr/` - User programs (like Program Files on Windows)
- `/opt/` - Optional third-party software
- `/srv/` - Service data (web files, FTP data)

### 🔌 **Mount Points**
- `/mnt/` - Temporary manual mounts
- `/media/` - Removable drives (USB, CD)

## Key Points to Remember

1. **/** is the root - everything starts here
2. **Don't confuse `/root` (admin home) with `/` (filesystem root)**
3. **/home** is where regular users live
4. **/var** grows over time (logs, databases)
5. **/usr** is like "Program Files" - where most software lives
6. **/etc** has ALL system configuration
7. **/proc** and `/sys` aren't real directories - they show live system info
8. **/tmp** gets cleaned automatically

## Common Paths You'll Use

```bash
/home/username/          # Your personal files
/etc/ssh/sshd_config     # SSH server configuration
/var/log/                # System logs
/usr/bin/                # Where most commands are
/tmp/                    # Temporary workspace
/dev/sda1                # First partition on first disk
```

## Permission Levels
- 📁 **Anyone can read**: `/usr`, `/bin`, `/lib`
- 🔒 **Root only**: `/root`, `/etc`, `/boot`, `/sbin`
- 👤 **User specific**: `/home/username`
- 🔄 **Everyone can write**: `/tmp`