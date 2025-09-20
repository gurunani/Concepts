# Linux Users & Permissions Management - Complete Guide

## 👥 User Management

### User Account Commands

#### Creating Users
```bash
# Basic user creation
useradd username              # Create user (minimal setup)
useradd -m username          # Create user with home directory
useradd -m -s /bin/bash username  # Create user with home dir and bash shell

# Advanced user creation
useradd -m -d /home/custom -s /bin/bash -c "Full Name" -G sudo,docker username
# -m: create home directory
# -d: specify home directory path
# -s: specify shell
# -c: add comment/full name
# -G: add to additional groups

# Create system user (for services)
useradd -r -s /bin/false -M serviceuser
# -r: system account
# -s /bin/false: no login shell
# -M: no home directory
```

#### Managing Users
```bash
# Modify existing users
usermod -aG sudo username     # Add user to sudo group
usermod -aG docker,www-data username  # Add to multiple groups
usermod -s /bin/zsh username  # Change default shell
usermod -d /new/home -m username  # Move home directory
usermod -l newname oldname    # Change username
usermod -L username           # Lock user account
usermod -U username           # Unlock user account

# Change user information
chfn username                 # Change full name, phone, etc.
chsh username                 # Change user's shell

# Delete users
userdel username              # Delete user (keep home directory)
userdel -r username           # Delete user and home directory
userdel -f username           # Force delete (even if logged in)
```

#### User Information
```bash
# View user information
id username                   # Show user ID and groups
id                           # Show current user info
whoami                       # Current username
who                          # Currently logged-in users
w                            # Detailed logged-in users info
users                        # Simple list of logged-in users
finger username              # Detailed user information (if installed)

# User lists
cat /etc/passwd              # All system users
getent passwd                # All users (includes LDAP/NIS)
cut -d: -f1 /etc/passwd      # List only usernames
awk -F: '$3>=1000 {print $1}' /etc/passwd  # List regular users (UID >= 1000)
```

### Group Management

#### Creating and Managing Groups
```bash
# Group creation
groupadd groupname           # Create new group
groupadd -g 1500 groupname   # Create group with specific GID

# Group modification
groupmod -n newname oldname  # Rename group
groupmod -g 2000 groupname   # Change group ID

# Delete groups
groupdel groupname           # Delete group

# Add/remove users from groups
gpasswd -a username groupname    # Add user to group
gpasswd -d username groupname    # Remove user from group
usermod -aG groupname username   # Add user to group (alternative)
```

#### Group Information
```bash
# View group information
groups username              # Show user's groups
groups                       # Show current user's groups
cat /etc/group              # All system groups
getent group                # All groups (includes LDAP/NIS)
grep groupname /etc/group   # Specific group info

# List group members
getent group groupname      # Show group members
members groupname           # Show group members (if available)
```

### Password Management

```bash
# Change passwords
passwd                      # Change current user password
passwd username             # Change another user's password (as root)
passwd -l username          # Lock user password
passwd -u username          # Unlock user password
passwd -d username          # Delete user password (dangerous!)

# Password policies
chage -l username           # View password aging info
chage -M 90 username        # Set password max age to 90 days
chage -m 7 username         # Set minimum password age to 7 days
chage -W 7 username         # Set warning period to 7 days
chage -E 2025-12-31 username # Set account expiration date

# Force password change at next login
chage -d 0 username
passwd -e username          # Alternative method
```

## 🔐 File Permissions & Ownership

### Understanding Permissions

#### Permission Types
```bash
# Permission symbols
r = read (4)     # View file content or list directory
w = write (2)    # Modify file or create/delete in directory
x = execute (1)  # Run file or enter directory

# Permission groups
u = user/owner   # File owner
g = group        # Group members
o = others       # Everyone else
a = all          # All users (u+g+o)

# Permission display format
-rwxr-xr--
# Position 1: File type (- = regular file, d = directory, l = link)
# Positions 2-4: Owner permissions (rwx)
# Positions 5-7: Group permissions (r-x)
# Positions 8-10: Others permissions (r--)
```

#### Octal (Numeric) Permissions
```bash
# Octal values
4 = read (r)
2 = write (w)  
1 = execute (x)

# Common permissions
755 = rwxr-xr-x  # Owner: rwx, Group: r-x, Others: r-x
644 = rw-r--r--  # Owner: rw-, Group: r--, Others: r--
600 = rw-------  # Owner: rw-, Group: ---, Others: ---
700 = rwx------  # Owner: rwx, Group: ---, Others: ---
777 = rwxrwxrwx  # Full permissions for everyone (dangerous!)
```

### Changing Permissions (chmod)

#### Symbolic Mode
```bash
# Add permissions
chmod u+x file              # Add execute for owner
chmod g+w file              # Add write for group
chmod o+r file              # Add read for others
chmod a+x file              # Add execute for all

# Remove permissions
chmod u-x file              # Remove execute for owner
chmod g-w file              # Remove write for group
chmod o-r file              # Remove read for others
chmod a-x file              # Remove execute for all

# Set exact permissions
chmod u=rwx file            # Set owner to rwx
chmod g=r file              # Set group to r only
chmod o= file               # Remove all permissions for others
chmod u=rwx,g=rx,o=r file   # Set multiple permissions

# Common combinations
chmod +x script.sh          # Make file executable for all
chmod -w file.txt           # Remove write permission for all
```

#### Octal Mode
```bash
# Set permissions with octal numbers
chmod 755 file              # rwxr-xr-x
chmod 644 file              # rw-r--r--
chmod 600 file              # rw-------
chmod 700 directory         # rwx------ for directories
chmod 777 file              # rwxrwxrwx (full permissions)

# Recursive permission changes
chmod -R 755 directory/     # Apply 755 to directory and all contents
chmod -R u+x directory/     # Add execute for owner recursively
```

### Changing Ownership (chown & chgrp)

```bash
# Change owner
chown newowner file         # Change owner only
chown newowner:newgroup file # Change owner and group
chown :newgroup file        # Change group only (alternative to chgrp)

# Change group
chgrp newgroup file         # Change group ownership

# Recursive ownership changes
chown -R user:group directory/    # Change ownership recursively
chgrp -R newgroup directory/      # Change group recursively

# Copy permissions from reference file
chmod --reference=reffile targetfile
chown --reference=reffile targetfile

# Examples
chown john:developers project/    # john owns, developers group
chown www-data:www-data /var/www/ # Web server ownership
chown -R mysql:mysql /var/lib/mysql/  # Database files
```

### Advanced Permissions

#### Special Permissions
```bash
# Setuid (SUID) - bit 4
chmod u+s file              # Set SUID bit
chmod 4755 file             # Set SUID with octal (4000 + 755)
# File executes with owner's permissions, not executor's

# Setgid (SGID) - bit 2
chmod g+s file              # Set SGID bit on file
chmod g+s directory         # Set SGID bit on directory
chmod 2755 directory        # Set SGID with octal (2000 + 755)
# Files: execute with group's permissions
# Directories: new files inherit directory's group

# Sticky bit - bit 1
chmod +t directory          # Set sticky bit
chmod 1755 directory        # Set sticky with octal (1000 + 755)
# Only owner can delete files in directory (like /tmp)

# View special permissions
ls -l file                  # SUID shows as 's' in owner execute
ls -ld directory            # SGID shows as 's' in group execute
                           # Sticky shows as 't' in others execute
```

#### Access Control Lists (ACLs)
```bash
# Check if filesystem supports ACLs
mount | grep acl
getfacl file                # View ACL permissions

# Set ACL permissions
setfacl -m u:username:rwx file    # Give user rwx permissions
setfacl -m g:groupname:rx file    # Give group rx permissions
setfacl -m o::r file              # Set others to read only

# Default ACLs for directories
setfacl -d -m u:username:rwx dir  # Default ACL for new files
setfacl -d -m g:groupname:rx dir  # Default ACL for group

# Remove ACL permissions
setfacl -x u:username file        # Remove user ACL
setfacl -b file                   # Remove all ACL permissions

# Copy ACLs
getfacl file1 | setfacl --set-file=- file2
```

### Default File Permissions (umask)

```bash
# View current umask
umask                       # Show current umask (octal)
umask -S                    # Show current umask (symbolic)

# Set umask
umask 022                   # Set umask to 022
umask u=rwx,g=rx,o=rx      # Set umask symbolically

# Calculate default permissions
# Files: 666 - umask = default file permissions
# Directories: 777 - umask = default directory permissions

# Examples with umask 022:
# New files: 666 - 022 = 644 (rw-r--r--)
# New directories: 777 - 022 = 755 (rwxr-xr-x)

# Common umask values
umask 022                   # Files: 644, Directories: 755
umask 002                   # Files: 664, Directories: 775
umask 077                   # Files: 600, Directories: 700 (restrictive)

# Set umask permanently
echo "umask 022" >> ~/.bashrc      # For current user
echo "umask 022" >> /etc/profile   # System-wide
```

## 🛡️ Sudo Configuration

### Basic Sudo Usage
```bash
# Run command as root
sudo command                # Run command with root privileges
sudo -u username command    # Run command as specific user
sudo -i                     # Switch to root shell
sudo su -                   # Switch to root user
sudo su - username          # Switch to specific user

# Sudo with environment
sudo -E command             # Preserve environment variables
sudo --preserve-env command # Alternative syntax

# List sudo privileges
sudo -l                     # List allowed commands for current user
sudo -l -U username         # List privileges for specific user
```

### Sudo Configuration (/etc/sudoers)

#### Editing Sudoers File
```bash
# ALWAYS use visudo to edit sudoers file
visudo                      # Edit /etc/sudoers safely
visudo -f /etc/sudoers.d/custom  # Edit custom sudoers file

# Sudoers file syntax
# User/Group Host=(RunAs) Commands
username ALL=(ALL) ALL     # User can run any command as any user
%groupname ALL=(ALL) ALL   # Group members can run any command
```

#### Sudoers Examples
```bash
# Basic user permissions
john ALL=(ALL) ALL          # John can run everything as root
mary ALL=(ALL) NOPASSWD: ALL # Mary doesn't need password

# Specific commands
alice ALL=(ALL) /usr/bin/systemctl, /usr/bin/service
bob ALL=(ALL) /bin/mount, /bin/umount

# Group permissions
%wheel ALL=(ALL) ALL        # wheel group has full sudo access
%admin ALL=(ALL) NOPASSWD: /usr/bin/apt-get, /usr/bin/apt

# Run as specific user
webadmin ALL=(www-data) ALL # Can run commands as www-data user
dbadmin ALL=(mysql) /usr/bin/mysql*

# Host-specific permissions
user1 webserver1=(ALL) /usr/bin/systemctl restart nginx
user2 ALL=(ALL) !/bin/su    # Can run all commands except su

# Command aliases
Cmnd_Alias SERVICES = /usr/bin/systemctl, /usr/sbin/service
Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping
%operators ALL=(ALL) SERVICES, NETWORKING

# User aliases  
User_Alias ADMINS = john, mary, alice
ADMINS ALL=(ALL) ALL
```

#### Sudoers Security Settings
```bash
# In /etc/sudoers
Defaults env_reset                    # Reset environment
Defaults mail_badpass                 # Mail failed attempts
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
Defaults timestamp_timeout=15         # Sudo timeout (minutes)
Defaults passwd_timeout=5             # Password prompt timeout
Defaults passwd_tries=3               # Number of password attempts
Defaults logfile="/var/log/sudo.log"  # Sudo log file
```

### Managing Sudo Access
```bash
# Add user to sudo group (Debian/Ubuntu)
usermod -aG sudo username
gpasswd -a username sudo

# Add user to wheel group (RHEL/CentOS)
usermod -aG wheel username
gpasswd -a username wheel

# Remove sudo access
gpasswd -d username sudo    # Remove from sudo group
visudo                      # Remove from sudoers file

# Temporary sudo access
# Create file in /etc/sudoers.d/
echo "tempuser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/tempuser
# Remember to delete when no longer needed
```

## 📁 File System Security

### File Attributes
```bash
# Extended attributes (on ext2/3/4 filesystems)
lsattr file                 # List file attributes
chattr +i file              # Make file immutable (cannot be modified/deleted)
chattr -i file              # Remove immutable attribute
chattr +a file              # Append-only (can only append data)
chattr +u file              # Undeletable (file can be deleted but contents saved)

# Common attributes
# i = immutable
# a = append only  
# u = undeletable
# s = secure deletion
# A = no atime updates
```

### Finding Files by Permissions
```bash
# Find files by permissions
find /path -perm 777        # Find files with exact permissions 777
find /path -perm -644       # Find files with at least 644 permissions
find /path -perm /u+w       # Find files writable by owner

# Find SUID/SGID files
find /usr -perm -4000       # Find SUID files
find /usr -perm -2000       # Find SGID files
find /usr -perm -6000       # Find files with both SUID and SGID

# Find world-writable files
find /home -perm -002       # Find world-writable files
find /tmp -perm -002 -type f # Find world-writable regular files

# Find files by owner
find /home -user john       # Find files owned by john
find /var -group www-data   # Find files owned by www-data group
find /tmp -nouser           # Find files with no owner
find /tmp -nogroup          # Find files with no group
```

### Security Best Practices

#### File Permission Guidelines
```bash
# Recommended permissions for common files/directories
/etc/passwd         644     # World-readable user database
/etc/shadow         600     # Root-only password hashes  
/etc/group          644     # World-readable group database
/etc/sudoers        440     # Root and sudo group readable
~/.ssh/             700     # SSH directory
~/.ssh/id_rsa       600     # Private SSH key
~/.ssh/id_rsa.pub   644     # Public SSH key
~/.ssh/authorized_keys 600  # Authorized SSH keys
/var/log/           755     # Log directories
/tmp                1777    # Temporary directory with sticky bit
/var/www/html       755     # Web content directories
/var/www/html/files 644     # Web content files
```

#### Regular Security Checks
```bash
# Check for suspicious SUID files
find /usr /bin /sbin -perm -4000 -ls

# Check for world-writable files
find / -path /proc -prune -o -perm -002 -type f -print 2>/dev/null

# Check for files without owners
find / -path /proc -prune -o -nouser -o -nogroup -print 2>/dev/null

# Check failed login attempts
grep "Failed password" /var/log/auth.log | tail -20

# Monitor sudo usage
grep sudo /var/log/auth.log | tail -10
tail -f /var/log/sudo.log   # If configured
```

## 🔧 User Environment Management

### User Profile Files
```bash
# System-wide profile files
/etc/profile            # System-wide environment settings
/etc/bash.bashrc        # System-wide bash settings
/etc/environment        # System-wide environment variables

# User-specific profile files
~/.profile              # Login shell environment (all shells)
~/.bashrc               # Bash-specific settings
~/.bash_profile         # Bash login shell settings  
~/.bash_logout          # Executed when bash login shell exits
~/.vimrc                # Vim editor settings
~/.ssh/config           # SSH client configuration
```

### Environment Variables
```bash
# View environment
env                     # Show all environment variables
printenv                # Show all environment variables
echo $HOME              # Show specific variable
echo $PATH              # Show PATH variable
echo $USER              # Show current user

# Set environment variables
export VARNAME=value    # Set variable for current session
export PATH=$PATH:/new/path  # Add to PATH

# Permanent environment variables
echo 'export VARNAME=value' >> ~/.bashrc     # For current user
echo 'VARNAME=value' >> /etc/environment     # System-wide
```

### Shell Configuration
```bash
# Change default shell
chsh -s /bin/bash username      # Set bash as default shell
chsh -s /bin/zsh username       # Set zsh as default shell

# View available shells
cat /etc/shells                 # List available shells
echo $SHELL                     # Current shell

# Shell-specific configurations
~/.bashrc                       # Bash configuration
~/.zshrc                        # Zsh configuration  
~/.config/fish/config.fish      # Fish shell configuration
```

## 📋 Quick Reference

### Essential Commands
| Task | Command |
|------|---------|
| Create user with home | `useradd -m username` |
| Add user to group | `usermod -aG groupname username` |
| Change password | `passwd username` |
| View user info | `id username` |
| Change file permissions | `chmod 755 filename` |
| Change owner | `chown user:group filename` |
| View permissions | `ls -l filename` |
| Edit sudoers safely | `visudo` |
| Check sudo privileges | `sudo -l` |
| Set file immutable | `chattr +i filename` |
| Find SUID files | `find /usr -perm -4000` |
| View current umask | `umask` |
| View file ACLs | `getfacl filename` |
| Set ACL permissions | `setfacl -m u:user:rwx filename` |

### Common Permission Patterns
```bash
# Files
644 (-rw-r--r--)  # Standard file permissions
600 (-rw-------)  # Private files (SSH keys, configs)
755 (-rwxr-xr-x)  # Executable files, scripts

# Directories  
755 (drwxr-xr-x)  # Standard directory permissions
700 (drwx------)  # Private directories
1777 (drwxrwxrwt) # Shared directories with sticky bit (/tmp)

# Special cases
4755 (-rwsr-xr-x) # SUID executable
2755 (drwxr-sr-x) # SGID directory
```