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

/cron.log        # Cron job execution logs
```

#### Application Logs
```bash
/var/log/apache2/        # Apache web server logs
  ├── access.log         # HTTP requests
  ├── error.log          # Apache errors
  └── other_error.log    # Virtual host specific logs

/var/log/nginx/          # Nginx web server logs
  ├── access.log         # HTTP requests  
  ├── error.log          # Nginx errors
  └── site-access.log    # Site-specific logs

/var/log/mysql/          # MySQL database logs
  ├── error.log          # MySQL errors
  ├── slow.log           # Slow query log
  └── general.log        # General query log

/var/log/postgresql/     # PostgreSQL logs
/var/log/redis/          # Redis logs
/var/log/docker/         # Docker daemon logs
```

#### Service-Specific Logs
```bash
/var/log/ssh/            # SSH connection logs
/var/log/mail.log        # Mail server logs
/var/log/ufw.log         # Firewall logs (Ubuntu)
/var/log/fail2ban.log    # Fail2ban intrusion detection
/var/log/cups/           # Print system logs
/var/log/apt/            # Package manager logs (Debian/Ubuntu)
  ├── history.log        # Package install/remove history
  └── term.log           # Detailed package operations
```

### 🔍 Reading Log Files

#### Basic Log Viewing Commands
```bash
# View entire log file
cat /var/log/syslog      # Display entire file (small files only)
less /var/log/syslog     # Page through file (q to quit)
more /var/log/syslog     # Basic pager

# View last lines (most recent entries)
tail /var/log/syslog           # Last 10 lines
tail -20 /var/log/syslog       # Last 20 lines
tail -f /var/log/syslog        # Follow live (Ctrl+C to stop)
tail -F /var/log/syslog        # Follow with file rotation handling

# View first lines (oldest entries)  
head /var/log/syslog           # First 10 lines
head -50 /var/log/syslog       # First 50 lines

# View specific line ranges
sed -n '100,200p' /var/log/syslog  # Lines 100-200
awk 'NR>=100 && NR<=200' /var/log/syslog  # Same with awk
```

#### Advanced Log Analysis
```bash
# Search in logs
grep "error" /var/log/syslog           # Find lines with "error"
grep -i "failed" /var/log/auth.log     # Case-insensitive search
grep -n "nginx" /var/log/syslog        # Show line numbers
grep -A 5 -B 5 "error" /var/log/syslog # 5 lines before/after match

# Multiple file search
grep "error" /var/log/*.log            # Search all .log files
grep -r "failed login" /var/log/       # Recursive search in directory

# Time-based filtering
grep "$(date '+%b %d')" /var/log/syslog    # Today's entries
grep "Dec 25" /var/log/syslog              # Specific date
grep "$(date -d yesterday '+%b %d')" /var/log/syslog  # Yesterday

# Count occurrences
grep -c "error" /var/log/syslog        # Count error lines
grep "failed" /var/log/auth.log | wc -l # Count failed attempts

# Extract specific fields
awk '{print $1, $2, $3, $5}' /var/log/syslog  # Print specific columns
cut -d' ' -f1-3,5 /var/log/syslog      # Cut specific fields
```

### 📊 Log Monitoring & Analysis

#### Real-time Log Monitoring
```bash
# Follow multiple logs simultaneously
tail -f /var/log/syslog /var/log/auth.log

# Monitor with colored output
tail -f /var/log/syslog | grep --color=always -E "(error|fail|warning)"

# Watch for specific patterns
watch 'tail -20 /var/log/syslog | grep error'

# Multi-file monitoring with multitail
multitail /var/log/syslog /var/log/auth.log /var/log/nginx/error.log
```

#### Log Analysis Tools
```bash
# System log analyzer
logwatch                    # Generate log summary report
logwatch --detail High      # Detailed report
logwatch --service sshd     # Specific service report

# Log file statistics
wc -l /var/log/syslog       # Count total lines
du -h /var/log/*.log        # Check log file sizes
ls -lah /var/log/ | grep log # List log files with sizes

# Find large log files
find /var/log -name "*.log" -size +100M  # Files larger than 100MB
find /var/log -name "*.log" -exec ls -lh {} \; | sort -k5 -hr
```

### 🗂️ Systemd Journal (journalctl)

#### Basic journalctl Usage
```bash
# View system journal
journalctl                  # All journal entries
journalctl -n 50           # Last 50 entries
journalctl -f              # Follow live journal
journalctl -r              # Reverse order (newest first)

# Filter by service
journalctl -u nginx        # Nginx service logs
journalctl -u ssh          # SSH service logs
journalctl -u mysql        # MySQL service logs
journalctl -u docker       # Docker service logs

# Filter by priority/level
journalctl -p err          # Error level and above
journalctl -p warning      # Warning level and above
journalctl -p debug        # Debug level and above
```

#### Advanced journalctl Filtering
```bash
# Time-based filtering
journalctl --since "2024-01-01"           # Since specific date
journalctl --since "1 hour ago"           # Last hour
journalctl --since yesterday              # Since yesterday
journalctl --until "2024-01-31"           # Until specific date
journalctl --since "09:00" --until "17:00" # Time range today

# Combine filters
journalctl -u nginx --since "1 hour ago" -p err  # Nginx errors last hour
journalctl -u mysql --since yesterday --until today  # MySQL logs yesterday

# Filter by user or process
journalctl _UID=1000       # Logs from specific user ID
journalctl _PID=1234       # Logs from specific process ID
journalctl _COMM=nginx     # Logs from specific command

# Output formats
journalctl -o json         # JSON format
journalctl -o short        # Short format (default)
journalctl -o verbose      # Verbose format with all fields
```

### 🧹 Log Rotation & Management

#### Log Rotation Configuration
```bash
# Logrotate configuration
/etc/logrotate.conf        # Main logrotate configuration
/etc/logrotate.d/          # Service-specific rotation configs

# Example logrotate config for custom app:
cat > /etc/logrotate.d/myapp << EOF
/var/log/myapp/*.log {
    daily                  # Rotate daily
    rotate 30             # Keep 30 old logs
    compress              # Compress old logs
    delaycompress         # Don't compress most recent old log
    missingok             # OK if log file doesn't exist
    create 644 root root  # Create new log with permissions
    postrotate
        systemctl reload myapp
    endscript
}
EOF

# Manual log rotation
logrotate -f /etc/logrotate.d/myapp  # Force rotation
logrotate -d /etc/logrotate.conf     # Debug/dry-run
```

#### Log Cleanup & Maintenance
```bash
# Clean old logs manually
find /var/log -name "*.log.*" -mtime +30 -delete  # Delete logs older than 30 days
find /var/log -name "*.gz" -mtime +90 -delete     # Delete compressed logs older than 90 days

# Check disk usage by logs
du -sh /var/log/*          # Size of each log directory
df -h /var/log             # Disk usage of log partition

# Truncate large log files
> /var/log/large-file.log  # Empty the file
truncate -s 0 /var/log/large-file.log  # Alternative method

# Archive logs
tar -czf logs-backup-$(date +%Y%m%d).tar.gz /var/log/*.log
```

### 🚨 Common Log Analysis Scenarios

#### Security Analysis
```bash
# Failed login attempts
grep "Failed password" /var/log/auth.log | tail -20
grep "authentication failure" /var/log/auth.log

# Successful logins
grep "Accepted password" /var/log/auth.log
grep "session opened" /var/log/auth.log

# SSH brute force detection
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

# Firewall blocks
grep "UFW BLOCK" /var/log/ufw.log
```

#### Web Server Analysis
```bash
# Apache/Nginx access analysis
tail -f /var/log/nginx/access.log

# Count requests by IP
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -10

# Find 404 errors
grep "404" /var/log/nginx/access.log

# Response code analysis
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
```

#### Application Debugging
```bash
# Application errors
grep -i "error\|exception\|fatal" /var/log/myapp/app.log

# Performance issues
grep -i "slow\|timeout\|performance" /var/log/myapp/app.log

# Database connection issues
grep -i "connection\|database\|mysql\|postgresql" /var/log/myapp/app.log
```

### 📋 Log Commands Quick Reference

| Task | Command |
|------|---------|
| View live logs | `tail -f /var/log/syslog` |
| Search in logs | `grep "error" /var/log/syslog` |
| Last 100 lines | `tail -100 /var/log/syslog` |
| Service logs | `journalctl -u nginx` |
| Live service logs | `journalctl -f -u nginx` |
| Today's logs | `journalctl --since today` |
| Error level logs | `journalctl -p err` |
| Log file sizes | `du -h /var/log/*.log` |
| Count log entries | `wc -l /var/log/syslog` |
| Clear log file | `> /var/log/file.log` |

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
| View live logs | `tail -f /var/log/syslog` |
| Search in logs | `grep "error" /var/log/syslog` |
| Service logs | `journalctl -u nginx` |
| Check network interfaces | `ip addr show` |