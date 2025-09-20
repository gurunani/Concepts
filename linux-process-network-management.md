# Linux Process & Network Management - Quick Reference

## 🔄 Process Management

### Basic Process Commands

```bash
# View processes
ps aux                    # All running processes (detailed)
ps -ef                    # All processes (different format)
ps -u username            # Processes for specific user
pstree                    # Process tree view

# Process monitoring
htop                      # Interactive process viewer (better than top)
top                       # Real-time process monitor
nload                     # Network load monitor
iostat                    # I/O statistics

# Process control
kill PID                  # Terminate process by ID
kill -9 PID              # Force kill process
killall process_name      # Kill all processes by name
pkill -f "pattern"       # Kill processes matching pattern

# Job control
jobs                      # List active jobs
bg                        # Put job in background
fg                        # Bring job to foreground
nohup command &          # Run command immune to hangups
```

### Process Types & Management

#### 🎯 Foreground Processes (Interactive)
```bash
# Running in foreground (blocks terminal)
python3 script.py         # Runs in foreground, terminal waits
nano file.txt            # Text editor blocks terminal
wget large-file.zip      # Download blocks terminal
ping google.com          # Continuous ping blocks terminal
top                      # System monitor blocks terminal

# Commands that run and finish
ls -la                   # List files (quick)
cat file.txt            # Display file content
grep "pattern" file.txt  # Search in file

# Bring background process to foreground
fg                       # Bring last job to foreground
fg %1                    # Bring job number 1 to foreground
fg %job_name            # Bring named job to foreground
```

#### 🔄 Background Processes (Non-blocking)
```bash
# Running processes in background (&)
python3 app.py &         # Run Python script in background
wget large-file.zip &    # Download file in background
ping google.com > ping.log 2>&1 &  # Ping in background with logging
sleep 300 &              # Long-running command in background

# Convert foreground to background
# Press Ctrl+Z to suspend, then:
bg                       # Continue suspended job in background
bg %1                    # Continue specific job in background

# Start processes immune to terminal closing
nohup python3 app.py &   # Process continues after logout
nohup long-script.sh > output.log 2>&1 &  # With output redirection

# Process management
jobs                     # List background jobs
jobs -l                  # List jobs with PIDs
disown %1               # Remove job from shell's job table
disown -a               # Remove all jobs from job table

# System services/daemons (always run in background)
systemctl start nginx    # Start web server service
systemctl start mysql    # Start database service
systemctl start docker   # Start container service

# Screen and tmux sessions (persistent background)
screen -S mysession      # Create named screen session
screen -r mysession      # Reattach to screen session
tmux new -s mysession    # Create named tmux session
tmux attach -t mysession # Attach to tmux session
```

### Process States & Control
```bash
# Process control signals
kill -STOP PID          # Pause/suspend process
kill -CONT PID          # Resume paused process
kill -TERM PID          # Graceful termination
kill -KILL PID          # Force kill (same as kill -9)

# Job control shortcuts
Ctrl+Z                  # Suspend foreground process
Ctrl+C                  # Terminate foreground process
Ctrl+D                  # End input (EOF)

# Check process status
ps aux | grep process_name
ps -ef | grep PID
pgrep -f "pattern"      # Find processes by pattern
```

### 📊 htop - Interactive Process Manager

```bash
htop                      # Launch htop

# htop Keyboard Shortcuts:
F1    # Help
F2    # Setup/Settings
F3    # Search process
F4    # Filter processes
F5    # Tree view
F6    # Sort by column
F9    # Kill process
F10   # Quit

# Navigation in htop:
↑↓    # Navigate processes
H     # Hide/show user processes
K     # Hide/show kernel threads
t     # Tree view toggle
u     # Filter by user
/     # Search
Space # Tag process
c     # Tag children
```

## 🌐 Network Management

### Network Information Commands

```bash
# Network interfaces
ip addr show              # Show all network interfaces
ip a                      # Short version
ifconfig                  # Legacy command (if available)
iwconfig                  # Wireless interfaces

# Network statistics
netstat -i               # Interface statistics
ip -s link               # Interface statistics (modern)

# Routing
ip route show            # Show routing table
route -n                 # Legacy routing table
```

### 🔍 netstat - Network Connections

```bash
# Basic netstat usage
netstat -lntp            # Most common: Listening TCP ports with PIDs

# Breaking down netstat -lntp:
# -l : Show only listening ports
# -n : Show numerical addresses instead of hosts
# -t : Show TCP connections
# -p : Show PID and process name
```

#### Detailed netstat Options
```bash
# TCP connections
netstat -tnp             # All TCP connections with PIDs
netstat -ltn             # Listening TCP ports (no PIDs)
netstat -atn             # All TCP connections

# UDP connections  
netstat -unp             # All UDP connections with PIDs
netstat -lun             # Listening UDP ports

# Combined options
netstat -tulpn           # TCP + UDP, listening + established
netstat -antp            # All TCP with PIDs
netstat -r               # Routing table

# Filter by port
netstat -lntp | grep :80     # Check if port 80 is listening
netstat -lntp | grep :3306   # Check MySQL port
netstat -lntp | grep :22     # Check SSH port
```

#### Common netstat Output Interpretation
```bash
# Example output of netstat -lntp:
Proto Recv-Q Send-Q Local Address    Foreign Address  State    PID/Program
tcp   0      0      0.0.0.0:22       0.0.0.0:*        LISTEN   1234/sshd
tcp   0      0      127.0.0.1:3306   0.0.0.0:*        LISTEN   5678/mysqld
tcp6  0      0      :::80            :::*             LISTEN   9012/nginx

# Meaning:
# 0.0.0.0:22    - SSH listening on all interfaces, port 22
# 127.0.0.1:3306 - MySQL listening only on localhost, port 3306  
# :::80         - Nginx listening on all IPv6 interfaces, port 80
```

### Alternative Network Commands (Modern)

```bash
# ss - Modern replacement for netstat
ss -lntp                 # Same as netstat -lntp (faster)
ss -tulpn                # TCP + UDP listening ports
ss -antp                 # All TCP connections

# lsof - List open files (including network)
lsof -i                  # All network connections
lsof -i :80              # Connections on port 80
lsof -i tcp:22           # TCP connections on port 22
lsof -u username         # Files opened by user
```

## ⚙️ System Configuration Files

### Network Configuration
```bash
# Network interfaces
/etc/network/interfaces     # Debian/Ubuntu network config
/etc/netplan/*.yaml         # Ubuntu 18+ netplan config
/etc/sysconfig/network-scripts/  # RHEL/CentOS network scripts

# DNS Configuration
/etc/resolv.conf           # DNS servers
/etc/hosts                 # Static hostname to IP mapping
/etc/nsswitch.conf         # Name resolution order

# Network services
/etc/services              # Port to service name mapping
/etc/protocols             # Protocol definitions
```

### System Service Configuration
```bash
# Systemd configurations
/etc/systemd/system/       # Custom systemd service files
/usr/lib/systemd/system/   # Default systemd service files

# Service-specific configs
/etc/nginx/                # Nginx configuration
/etc/apache2/              # Apache configuration  
/etc/mysql/                # MySQL configuration
/etc/ssh/sshd_config       # SSH server configuration

# Process limits
/etc/security/limits.conf  # User process limits
/etc/systemd/system.conf   # Systemd limits
```

## 🔧 Common Troubleshooting Scenarios

### Check What's Running on a Port
```bash
# Method 1: netstat
netstat -lntp | grep :8080

# Method 2: lsof  
lsof -i :8080

# Method 3: ss
ss -lntp | grep :8080
```

### Find Resource-Heavy Processes
```bash
# CPU usage
htop                      # Interactive view
ps aux --sort=-%cpu | head -10  # Top 10 CPU users

# Memory usage  
ps aux --sort=-%mem | head -10  # Top 10 memory users
free -h                   # System memory usage
```

### Service Management
```bash
# Check if service is running
systemctl is-active nginx
systemctl status nginx

# Check service logs
journalctl -u nginx       # Service logs
journalctl -f -u nginx    # Follow logs real-time
tail -f /var/log/nginx/access.log  # Traditional log following
```

### Network Connectivity Testing
```bash
# Basic connectivity
ping google.com           # Test internet connectivity
ping -c 4 192.168.1.1     # Test local gateway

# Port connectivity
telnet google.com 80      # Test if port is reachable
nc -zv google.com 80      # Netcat port check
curl -I http://google.com # HTTP connectivity test

# DNS testing
nslookup google.com       # DNS lookup
dig google.com            # Detailed DNS query
```

## 📋 Quick Command Cheat Sheet

| Task | Command |
|------|---------|
| View all processes | `ps aux` |
| Interactive process monitor | `htop` |
| Kill process by PID | `kill 1234` |
| Check listening ports | `netstat -lntp` |
| Modern port check | `ss -lntp` |
| Check specific port | `lsof -i :80` |
| Service status | `systemctl status nginx` |
| Start/stop service | `systemctl start/stop nginx` |
| Follow log files | `tail -f /var/log/file.log` |
| Check network interfaces | `ip addr show` |