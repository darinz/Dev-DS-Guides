# Linux and Unix Command Line Reference

<p align="center">
  <img src="https://img.shields.io/badge/Linux-000000?style=flat-square&logo=linux&logoColor=white" alt="Linux" />
  <img src="https://img.shields.io/badge/Unix-4D4D4D?style=flat-square&logo=unix&logoColor=white" alt="Unix" />
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white" alt="Bash" />
  <img src="https://img.shields.io/badge/Shell_Scripting-FF6C37?style=flat-square&logo=shell&logoColor=white" alt="Shell Scripting" />
  <img src="https://img.shields.io/badge/System_Admin-FF6600?style=flat-square&logo=server&logoColor=white" alt="System Admin" />
  <img src="https://img.shields.io/badge/CLI-000000?style=flat-square&logo=terminal&logoColor=white" alt="CLI" />
</p>

A comprehensive reference for Linux and Unix command-line operations, including basic commands, advanced system administration, shell scripting, and troubleshooting.

---

## Table of Contents

1. [File and Directory Operations](#file-and-directory-operations)
2. [File Viewing and Manipulation](#file-viewing-and-manipulation)
3. [Text Processing and Editing](#text-processing-and-editing)
4. [User and Permission Management](#user-and-permission-management)
5. [Networking](#networking)
6. [Package Management](#package-management)
7. [Process Management](#process-management)
8. [Disk Usage and System Monitoring](#disk-usage-and-system-monitoring)
9. [Searching and Filtering](#searching-and-filtering)
10. [Archiving and Compression](#archiving-and-compression)
11. [System Services](#system-services)
12. [Shell Scripting](#shell-scripting)
13. [Security Commands](#security-commands)
14. [Troubleshooting](#troubleshooting)
15. [Performance Monitoring](#performance-monitoring)

---

## File and Directory Operations

### Basic File Operations

```bash
# List directory contents
ls                    # Basic listing
ls -l                 # Long format with details
ls -la                # Long format including hidden files
ls -lh                # Human-readable file sizes
ls -lt                # Sort by modification time
ls -ltr               # Sort by modification time (reverse)
ls -R                 # Recursive listing
ls -d */              # List only directories

# Examples:
ls -la /home/user
ls -lh *.txt
ls -lt | head -10
```

### Directory Navigation

```bash
# Change directory
cd                    # Go to home directory
cd /path/to/dir       # Go to specific directory
cd ..                 # Go to parent directory
cd -                  # Go to previous directory
cd ~                  # Go to home directory

# Print working directory
pwd                   # Show current directory
pwd -P                # Show physical directory (resolve symlinks)

# Examples:
cd /var/log
cd ~/Documents
cd ../src
```

### Creating and Removing

```bash
# Create directories
mkdir dirname         # Create single directory
mkdir -p dir1/dir2    # Create nested directories
mkdir -m 755 dirname  # Create with specific permissions

# Remove directories
rmdir dirname         # Remove empty directory
rm -r dirname         # Remove directory and contents
rm -rf dirname        # Force remove (no confirmation)

# Examples:
mkdir -p ~/projects/myapp/{src,test,docs}
rm -rf /tmp/old_files
```

### File Operations

```bash
# Create files
touch filename        # Create empty file or update timestamp
touch -t 202312251200 filename  # Set specific timestamp

# Copy files and directories
cp file1 file2        # Copy file
cp -r dir1 dir2       # Copy directory recursively
cp -p file1 file2     # Preserve attributes
cp -v file1 file2     # Verbose output

# Move and rename
mv oldname newname    # Rename file
mv file1 dir1/        # Move file to directory
mv -i file1 file2     # Interactive (ask before overwrite)

# Remove files
rm filename           # Remove file
rm -f filename        # Force remove (no confirmation)
rm -i filename        # Interactive removal

# Examples:
cp -rv /home/user/documents /backup/
mv old_project new_project
rm -i *.tmp
```

### File Attributes

```bash
# Change file permissions
chmod 755 file        # Numeric permissions
chmod u+x file        # Symbolic permissions
chmod -R 644 dir/     # Recursive permission change

# Change ownership
chown user:group file # Change owner and group
chown user file       # Change owner only
chgrp group file      # Change group only

# Examples:
chmod +x script.sh
chown -R www-data:www-data /var/www/
```

---

## File Viewing and Manipulation

### Basic File Viewing

```bash
# Display file contents
cat file              # Display entire file
cat -n file           # Display with line numbers
cat -A file           # Show non-printing characters

# View file contents page by page
less file             # Interactive viewer (preferred)
more file             # Simple pager
most file             # Enhanced pager

# View beginning/end of file
head file             # First 10 lines
head -n 20 file       # First 20 lines
tail file             # Last 10 lines
tail -n 20 file       # Last 20 lines
tail -f file          # Follow file (real-time)

# Examples:
cat /etc/passwd
less /var/log/syslog
tail -f /var/log/nginx/access.log
```

### Advanced File Viewing

```bash
# View file with syntax highlighting
highlight file        # Syntax highlighting
pygmentize file       # Python-based highlighter
bat file              # Modern cat with syntax highlighting

# View binary files
hexdump -C file       # Hex dump
strings file          # Extract readable strings
file filename         # Determine file type

# View compressed files
zcat file.gz          # View gzipped file
bzcat file.bz2        # View bzipped file
xzcat file.xz         # View xz compressed file

# Examples:
file unknown_file
strings /bin/ls
zcat log.gz | grep error
```

### File Comparison

```bash
# Compare files
diff file1 file2      # Show differences
diff -u file1 file2   # Unified diff format
diff -r dir1 dir2     # Compare directories recursively

# Compare files side by side
sdiff file1 file2     # Side-by-side comparison
vimdiff file1 file2   # Vim-based diff

# Examples:
diff -u old_version.txt new_version.txt
diff -r /backup/ /current/
```

---

## Text Processing and Editing

### Text Editors

```bash
# Simple editors
nano file             # User-friendly editor
pico file             # Simple editor (deprecated)

# Advanced editors
vim file              # Vi improved
emacs file            # Emacs editor
gedit file            # GUI editor (if available)

# Examples:
nano /etc/hosts
vim ~/.bashrc
```

### Text Processing with sed

```bash
# Stream editor for text transformation
sed 's/old/new/' file           # Replace first occurrence
sed 's/old/new/g' file          # Replace all occurrences
sed 's/old/new/g' file > newfile # Save to new file
sed -i 's/old/new/g' file       # Edit file in place

# Examples:
sed 's/error/ERROR/g' log.txt
sed -i 's/127.0.0.1/localhost/g' /etc/hosts
sed '/^#/d' config.txt          # Remove comment lines
```

### Text Processing with awk

```bash
# Pattern scanning and processing
awk '{print $1}' file           # Print first field
awk '{print $1, $3}' file       # Print first and third fields
awk '$1 > 100' file             # Print lines where first field > 100
awk 'NR==1,NR==10' file         # Print lines 1-10

# Examples:
awk '{print $1}' /etc/passwd    # Print usernames
awk '$3 > 1000' /etc/passwd     # Print users with UID > 1000
awk '{sum += $1} END {print sum}' numbers.txt  # Sum first column
```

### Advanced Text Processing

```bash
# Sort and unique
sort file              # Sort lines
sort -n file           # Numeric sort
sort -r file           # Reverse sort
uniq file              # Remove consecutive duplicates
sort file | uniq       # Sort and remove all duplicates

# Cut and paste
cut -d: -f1 file       # Cut first field (colon delimiter)
cut -c1-10 file        # Cut characters 1-10
paste file1 file2      # Merge files side by side

# Examples:
sort -n -k2 data.txt   # Sort by second column numerically
cut -d: -f1,3 /etc/passwd | sort
```

---

## User and Permission Management

### User Management

```bash
# User information
whoami                # Show current user
id                    # Show user and group IDs
id username           # Show specific user info
who                   # Show logged in users
w                     # Show detailed user info

# User creation and modification
adduser username      # Interactive user creation
useradd username      # Non-interactive user creation
usermod -aG group username  # Add user to group
deluser username      # Delete user (Debian/Ubuntu)
userdel username      # Delete user (RedHat/CentOS)

# Password management
passwd                # Change current user password
passwd username       # Change specific user password
chpasswd              # Batch password change

# Examples:
adduser john
usermod -aG sudo john
passwd john
```

### Group Management

```bash
# Group operations
groups                # Show current user groups
groups username       # Show specific user groups
addgroup groupname    # Create group
groupadd groupname    # Create group (RedHat/CentOS)
delgroup groupname    # Delete group
groupdel groupname    # Delete group (RedHat/CentOS)

# Examples:
addgroup developers
usermod -aG developers john
```

### Permission Management

```bash
# File permissions
ls -l file            # View permissions
chmod 755 file        # Set permissions (rwxr-xr-x)
chmod u+x file        # Add execute for user
chmod g-w file        # Remove write for group
chmod o-r file        # Remove read for others

# Permission examples:
chmod 644 file        # rw-r--r-- (user read/write, group/others read)
chmod 755 script.sh   # rwxr-xr-x (user all, group/others read/execute)
chmod 600 private.txt # rw------- (user read/write only)

# Ownership
chown user:group file # Change owner and group
chown user file       # Change owner only
chgrp group file      # Change group only

# Examples:
chmod +x script.sh
chown -R www-data:www-data /var/www/
chmod 600 ~/.ssh/id_rsa
```

### Advanced Permissions

```bash
# Special permissions
chmod +s file         # Set SUID/SGID
chmod +t dir          # Set sticky bit
chmod 4755 file       # SUID (user executes as owner)
chmod 2755 file       # SGID (group executes as group owner)

# Access Control Lists (ACL)
setfacl -m u:user:rw file    # Add ACL for user
setfacl -m g:group:r file    # Add ACL for group
getfacl file                 # View ACLs

# Examples:
chmod 4755 /usr/bin/passwd   # SUID for password change
setfacl -m u:john:rw project.txt
```

---

## Networking

### Network Configuration

```bash
# Network interfaces
ip addr               # Show IP addresses
ip addr show          # Detailed interface info
ip link show          # Show interface status
ifconfig              # Legacy interface config (deprecated)

# Network configuration
ip addr add 192.168.1.100/24 dev eth0    # Add IP address
ip link set eth0 up                       # Enable interface
ip link set eth0 down                     # Disable interface

# Examples:
ip addr show eth0
ip addr add 10.0.0.5/24 dev eth0
```

### Network Connectivity

```bash
# Ping and connectivity
ping hostname         # Test connectivity
ping -c 4 hostname    # Send 4 packets
ping -I eth0 hostname # Use specific interface
ping6 hostname        # IPv6 ping

# Traceroute
traceroute hostname   # Show network path
traceroute -n hostname # No DNS resolution
mtr hostname          # Continuous traceroute

# Examples:
ping -c 4 google.com
traceroute 8.8.8.8
```

### Network Services

```bash
# Port scanning and services
netstat -tuln         # Show listening ports
ss -tuln              # Modern alternative to netstat
lsof -i               # Show open network connections
nmap localhost        # Network scanner

# Service status
systemctl status service    # Check service status
systemctl start service     # Start service
systemctl stop service      # Stop service
systemctl restart service   # Restart service

# Examples:
netstat -tuln | grep :80
systemctl status nginx
```

### File Transfer

```bash
# Download files
wget url              # Download file
wget -c url           # Continue interrupted download
wget -r url           # Recursive download
curl -O url           # Download with curl
curl -o filename url  # Download with custom name

# Secure file transfer
scp file user@host:/path    # Copy file to remote
scp user@host:/path file    # Copy file from remote
rsync -av src/ dest/        # Synchronize directories
rsync -avz src/ user@host:/dest/  # Sync to remote

# Examples:
wget https://example.com/file.zip
scp config.txt john@server:/home/john/
rsync -avz /backup/ user@backup-server:/backups/
```

### SSH and Remote Access

```bash
# SSH connections
ssh user@host         # Connect to remote host
ssh -p 2222 user@host # Connect on specific port
ssh -X user@host      # Enable X11 forwarding
ssh -L 8080:localhost:80 user@host  # Port forwarding

# SSH key management
ssh-keygen -t rsa     # Generate SSH key
ssh-copy-id user@host # Copy key to remote host
ssh-add ~/.ssh/id_rsa # Add key to agent

# Examples:
ssh john@192.168.1.100
ssh -L 3306:localhost:3306 user@database-server
```

### cURL (HTTP/FTP Client)

cURL is a powerful tool for transferring data with URLs. It supports HTTP, HTTPS, FTP, SFTP, and many other protocols.

#### Basic cURL Commands

```bash
# Simple GET request
curl http://google.com:80
curl -L google.com                    # Follow redirects
curl -o google google.com             # Save output to file

# Basic web requests
curl "google.com"                     # GET request
curl "www.google.com/search?q=foo"   # GET with parameters
```

#### POST Requests

```bash
# POST with data (-d flag)
curl http://example.com/cgi-bin/script.pl -d realname=sam -d hatesanchovies=true

# POST with form data (-F flag)
curl http://example.com/cgi-bin/script.pl -F picture=@.zshrc

# URL encoding
curl http://example.com/cgi-bin/script.pl --data-urlencode "foo=&bar"

# Note: Can't mix -d and -F flags (different types)
```

#### Customizing Requests

```bash
# Show only headers
curl stanford.edu/~darinz -I

# Verbose output
curl stanford.edu/~darinz -v

# Custom headers
curl stanford.edu/~darinz -H "Foo: bar"

# Custom user agent
curl whatismyipaddress.com -A "Mozilla/4.05"

# Follow redirects
curl -L http://example.com
```

#### Advanced cURL Features

```bash
# Generate C library code
curl --libcurl foo.c google.com

# Download with progress
curl -# -O https://example.com/file.zip

# Resume download
curl -C - -O https://example.com/file.zip

# Authentication
curl -u username:password https://example.com/api
curl --user username:password https://example.com/api

# Examples:
curl -v -H "Accept: application/json" https://api.github.com/users/octocat
curl -u myuser:mypass https://example.com/secure-area
```

#### cURL for FTP/SFTP

```bash
# FTP download
curl -u username:password ftp://ftp.example.com/file.txt -o localfile.txt

# FTP upload
curl -T localfile.txt ftp://ftp.example.com/ -u username:password

# SFTP (requires libssh2)
curl -u username sftp://host.com/file.txt -o localfile.txt

# Examples:
curl -u ftpuser:ftppass ftp://ftp.gnu.org/gnu/wget/wget-1.21.3.tar.gz
curl -T backup.tar.gz ftp://backup-server/ -u backupuser:backuppass
```

---

## Package Management

### Debian/Ubuntu (apt)

```bash
# Update package lists
apt update            # Update package index
apt upgrade           # Upgrade installed packages
apt full-upgrade      # Upgrade with dependency changes

# Package installation
apt install package   # Install package
apt install package1 package2  # Install multiple packages
apt install --reinstall package  # Reinstall package

# Package removal
apt remove package    # Remove package
apt purge package     # Remove package and config files
apt autoremove        # Remove unused packages

# Package information
apt search package    # Search for packages
apt show package      # Show package details
apt list --installed  # List installed packages
apt list --upgradable # List upgradable packages

# Examples:
apt update && apt upgrade
apt install nginx mysql-server
apt search python3
```

### RedHat/CentOS (yum/dnf)

```bash
# Update system
yum update            # Update all packages
dnf update            # Modern alternative to yum

# Package installation
yum install package   # Install package
dnf install package   # Install package (dnf)
yum groupinstall "Development Tools"  # Install group

# Package removal
yum remove package    # Remove package
yum groupremove "Development Tools"   # Remove group

# Package information
yum search package    # Search for packages
yum info package      # Show package details
yum list installed    # List installed packages

# Examples:
yum update
yum install httpd
yum groupinstall "Development Tools"
```

### Other Package Managers

```bash
# Snap packages (Ubuntu)
snap install package  # Install snap package
snap list             # List installed snaps
snap refresh package  # Update snap package

# Flatpak packages
flatpak install package  # Install flatpak package
flatpak list            # List installed flatpaks
flatpak update          # Update flatpak packages

# Examples:
snap install code
flatpak install org.gimp.GIMP
```

---

## Process Management

### Process Information

```bash
# List processes
ps                    # Show current shell processes
ps aux                # Show all processes
ps -ef                # Show all processes (alternative)
ps aux | grep process # Find specific process

# Process details
ps -p PID             # Show specific process
ps -o pid,ppid,cmd    # Custom output format
ps aux --sort=-%mem   # Sort by memory usage
ps aux --sort=-%cpu   # Sort by CPU usage

# Examples:
ps aux | grep nginx
ps -o pid,ppid,cmd --forest
```

### Interactive Process Management

```bash
# Top-like tools
top                   # Interactive process viewer
htop                  # Enhanced top (if installed)
iotop                 # I/O monitoring
nethogs               # Network usage by process

# Top commands (while in top):
# k - kill process
# r - renice process
# h - help
# q - quit
# 1 - toggle CPU cores
# m - toggle memory display

# Examples:
top -p $(pgrep nginx)
htop -u www-data
```

### Process Control

```bash
# Kill processes
kill PID              # Send TERM signal
kill -9 PID           # Send KILL signal
kill -HUP PID         # Send HUP signal
killall process_name  # Kill all processes by name
pkill process_name    # Kill processes by name
pgrep process_name    # Find process IDs by name

# Process priority
nice -n 10 command    # Run command with lower priority
renice 10 PID         # Change priority of running process

# Background processes
command &             # Run command in background
nohup command &       # Run command immune to hangups
screen                # Start screen session
tmux                  # Start tmux session

# Examples:
kill -9 $(pgrep firefox)
nice -n 10 ./long_task.sh
nohup ./backup.sh > backup.log 2>&1 &
```

### Job Control

```bash
# Job management
jobs                  # List background jobs
fg %1                 # Bring job 1 to foreground
bg %1                 # Resume job 1 in background
kill %1               # Kill job 1

# Examples:
./long_task.sh &
jobs
fg %1
```

---

## Disk Usage and System Monitoring

### Disk Usage

```bash
# Disk space
df -h                 # Human-readable disk usage
df -i                 # Inode usage
du -sh directory      # Directory size
du -h --max-depth=2   # Directory size with depth limit

# Find large files
find /path -type f -size +100M  # Files larger than 100MB
find /path -type f -size +1G    # Files larger than 1GB
du -ah /path | sort -hr | head -10  # Top 10 largest items

# Examples:
df -h /
du -sh /home/* | sort -hr
find /var/log -type f -size +100M
```

### System Information

```bash
# System information
uname -a              # All system information
uname -r              # Kernel release
cat /etc/os-release   # OS information
lscpu                 # CPU information
free -h               # Memory usage
vmstat 1 5            # Virtual memory stats

# Hardware information
lshw                  # List hardware
lsblk                 # List block devices
fdisk -l              # Disk partition information
blkid                 # Block device attributes

# Examples:
uname -a
free -h
lscpu | grep "Model name"
```

### System Monitoring

```bash
# Real-time monitoring
top                   # Process and system monitoring
htop                  # Enhanced top
iotop                 # I/O monitoring
iftop                 # Network monitoring
nethogs               # Network usage by process

# System logs
dmesg                 # Kernel ring buffer
journalctl            # Systemd journal
tail -f /var/log/syslog  # Follow system log
tail -f /var/log/auth.log  # Follow auth log

# Examples:
journalctl -f
tail -f /var/log/nginx/error.log
```

---

## Searching and Filtering

### File Searching

```bash
# Find files
find /path -name "*.txt"           # Find files by name
find /path -iname "*.txt"          # Case-insensitive search
find /path -type f -name "*.txt"   # Only files
find /path -type d -name "dir*"    # Only directories
find /path -mtime -7               # Modified in last 7 days
find /path -size +100M             # Larger than 100MB

# Advanced find examples
find /home -user john -type f      # Files owned by john
find /var/log -name "*.log" -exec rm {} \;  # Remove log files
find . -name "*.tmp" -delete       # Delete temp files

# Examples:
find /home -name "*.pdf" -type f
find /var/log -mtime +30 -name "*.log"
```

### Text Searching

```bash
# grep variations
grep "pattern" file                # Basic search
grep -i "pattern" file             # Case-insensitive
grep -v "pattern" file             # Invert match
grep -r "pattern" directory        # Recursive search
grep -l "pattern" directory        # List files with matches
grep -n "pattern" file             # Show line numbers

# Advanced grep
grep -E "pattern1|pattern2" file   # Extended regex
grep -A 3 "pattern" file           # Show 3 lines after match
grep -B 3 "pattern" file           # Show 3 lines before match
grep -C 3 "pattern" file           # Show 3 lines around match

# Examples:
grep -r "error" /var/log/
grep -i "password" config.txt
grep -A 2 -B 2 "exception" log.txt
```

### Content Searching

```bash
# locate (faster than find for file names)
locate filename       # Find file by name
locate -i filename    # Case-insensitive
updatedb              # Update locate database

# which and whereis
which command         # Show command location
whereis command       # Show command and related files

# Examples:
locate nginx.conf
which python3
whereis git
```

### I/O Redirection and Pipes

```bash
# Basic redirection
command > file                    # Redirect output to file (overwrite)
command >> file                   # Redirect output to file (append)
command < file                    # Redirect input from file
command 2> file                   # Redirect error output to file
command 2>> file                  # Redirect error output to file (append)

# Combining output and error
command > file 2>&1               # Redirect both output and error to file
command &> file                   # Alternative syntax (bash)
command > file 2>&1               # Same as above

# Examples:
grep cat temp.txt > cat_temp.txt
grep cat temp.txt >> cat_append_temp.txt
awesome 2>> temp                  # Redirect error output
./read-input.py < input.txt       # Use input from file
```

### Advanced I/O Operations

```bash
# Tee command (output to stdout and file)
cat file | tee output.txt         # Output to both terminal and file
cat words | tee tee_words         # Save output to file
ping google.org | tee ping_google # Save ping output

# Here documents
cat << EOF > file.txt
Line 1
Line 2
EOF

# Process substitution
diff <(sort file1) <(sort file2)  # Compare sorted files
cat <(echo "Hello") <(echo "World") # Combine command outputs

# Examples:
ls -la | tee directory_listing.txt
cat << 'EOF' > script.sh
#!/bin/bash
echo "Hello World"
EOF
```

---

## Archiving and Compression

### Tar Archives

```bash
# Create archives
tar -cvf archive.tar files/        # Create tar archive
tar -czvf archive.tar.gz files/    # Create gzipped archive
tar -cjvf archive.tar.bz2 files/   # Create bzipped archive
tar -cJvf archive.tar.xz files/    # Create xz compressed archive

# Extract archives
tar -xvf archive.tar               # Extract tar archive
tar -xzvf archive.tar.gz           # Extract gzipped archive
tar -xjvf archive.tar.bz2          # Extract bzipped archive
tar -xJvf archive.tar.xz           # Extract xz compressed archive

# List archive contents
tar -tvf archive.tar               # List contents
tar -tzvf archive.tar.gz           # List gzipped archive contents

# Examples:
tar -czvf backup-$(date +%Y%m%d).tar.gz /home/user/
tar -xzvf project.tar.gz
tar -tvf archive.tar | grep "\.txt$"
```

### Advanced Tar Operations

```bash
# Split large archives into chunks
tar chzvf - directory/ | split -b 200M - archive_name.tgz.
# Creates: archive_name.tgz.aa, archive_name.tgz.ab, etc.

# Split with different chunk sizes
tar chzvf - * | split -b 80M - "backup.tgz."

# Combine split archives
cat archive_name.tgz.* | tar xvzf -
cat backup.tgz.* | tar xvzf -

# Examples:
tar chzvf - ../../* | split -b 200M - ../../"course1.tgz."
tar chzvf - * | split -b 80M - "C5.tgz."
cat C1.tgz.* | tar xvzf -
```

### Compression Tools

```bash
# gzip compression
gzip file             # Compress file (creates file.gz)
gunzip file.gz        # Decompress file
gzip -d file.gz       # Alternative decompression
zcat file.gz          # View compressed file

# bzip2 compression
bzip2 file            # Compress file (creates file.bz2)
bunzip2 file.bz2      # Decompress file
bzcat file.bz2        # View compressed file

# xz compression
xz file               # Compress file (creates file.xz)
unxz file.xz          # Decompress file
xzcat file.xz         # View compressed file

# Examples:
gzip -9 large_file    # Maximum compression
bzip2 -k file         # Keep original file
xz -e file            # Extreme compression
```

### Zip Archives

```bash
# Create zip archives
zip archive.zip file1 file2        # Create zip archive
zip -r archive.zip directory/      # Recursive zip
zip -P password archive.zip file   # Password protected

# Extract zip archives
unzip archive.zip                  # Extract to current directory
unzip archive.zip -d directory/    # Extract to specific directory
unzip -l archive.zip               # List contents

# Examples:
zip -r backup.zip /home/user/
unzip -d /tmp/ archive.zip
```

---

## System Services

### Systemd Management

```bash
# Service management
systemctl start service            # Start service
systemctl stop service             # Stop service
systemctl restart service          # Restart service
systemctl reload service           # Reload service
systemctl status service           # Check service status
systemctl enable service           # Enable service at boot
systemctl disable service          # Disable service at boot

# Service information
systemctl list-units --type=service  # List all services
systemctl list-units --failed        # List failed services
systemctl show service               # Show service details

# Examples:
systemctl start nginx
systemctl enable mysql
systemctl status ssh
```

### Service Logs

```bash
# Journal management
journalctl                           # Show all logs
journalctl -u service                # Show service logs
journalctl -f                        # Follow logs
journalctl --since "1 hour ago"      # Show recent logs
journalctl --since "2023-12-25"     # Show logs since date

# Log rotation
logrotate -d /etc/logrotate.conf    # Test log rotation
logrotate -f /etc/logrotate.conf    # Force log rotation

# Examples:
journalctl -u nginx -f
journalctl --since "1 day ago" | grep error
```

### Cron Jobs

```bash
# Crontab management
crontab -l                          # List current user's crontab
crontab -e                          # Edit current user's crontab
crontab -r                          # Remove current user's crontab

# System crontab
sudo crontab -l                     # List root's crontab
sudo crontab -e                     # Edit root's crontab

# Cron examples:
# 0 2 * * * /path/to/backup.sh      # Daily at 2 AM
# */15 * * * * /path/to/check.sh    # Every 15 minutes
# 0 0 1 * * /path/to/monthly.sh     # Monthly on 1st

# Examples:
crontab -l
echo "0 2 * * * /home/user/backup.sh" | crontab -
```

---

## Shell Scripting

### Basic Scripting

```bash
#!/bin/bash
# This is a comment

# Variables
name="John"
echo "Hello, $name"

# Command substitution
current_date=$(date)
echo "Current date: $current_date"

# Arithmetic
sum=$((5 + 3))
echo "Sum: $sum"

# Examples:
# Save as script.sh, then: chmod +x script.sh
# Run with: ./script.sh
```

### Control Structures

```bash
#!/bin/bash

# If statement
if [ -f "file.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi

# For loop
for i in {1..5}; do
    echo "Number: $i"
done

# While loop
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done

# Case statement
case $1 in
    "start")
        echo "Starting service"
        ;;
    "stop")
        echo "Stopping service"
        ;;
    *)
        echo "Usage: $0 {start|stop}"
        ;;
esac
```

### Advanced Scripting

```bash
#!/bin/bash

# Functions
backup_files() {
    local source_dir="$1"
    local backup_dir="$2"
    
    if [ ! -d "$source_dir" ]; then
        echo "Source directory does not exist"
        return 1
    fi
    
    mkdir -p "$backup_dir"
    cp -r "$source_dir"/* "$backup_dir/"
    echo "Backup completed"
}

# Error handling
set -e  # Exit on error
trap 'echo "Error occurred. Exiting."' ERR

# Command line arguments
while getopts "a:b:" opt; do
    case $opt in
        a) arg1="$OPTARG" ;;
        b) arg2="$OPTARG" ;;
        *) echo "Usage: $0 -a arg1 -b arg2" ;;
    esac
done

# Examples:
backup_files /home/user /backup/
```

### Practical Bash Script Examples

#### Basic Script Structure

```bash
#!/bin/bash
echo "Hello World"

# Run script
bash script.sh
# or
chmod a+x script.sh
./script.sh
```

#### Arithmetic Operations

```bash
#!/bin/bash

# Add two numeric values
((sum=25+35))

# Print the result
echo $sum

# Alternative syntax
sum=$((25 + 35))
echo "Sum: $sum"
```

#### Multi-line Comments

```bash
#!/bin/bash
: '
The following script calculates
the square value of the number, 5.
'
((area=5*5))
echo $area

# Alternative comment style
<<'COMMENT'
This is another way to create
multi-line comments in bash.
COMMENT
```

#### String Manipulation

```bash
#!/bin/bash

# Combine string variables
string1="Linux"
string2="Hint"
echo "$string1$string2"

# String concatenation with +
string3=$string1+$string2
string3+=" is a good tutorial blog site"
echo $string3

# Examples:
# Output: LinuxHint
# Output: Linux+Hint is a good tutorial blog site
```

#### Advanced Scripting

---

## Security Commands

### User Security

```bash
# Password policies
passwd -S username    # Check password status
chage -l username     # Show password aging info
chage -M 90 username  # Set max password age to 90 days

# Account locking
passwd -l username    # Lock account
passwd -u username    # Unlock account
usermod -L username   # Lock account (alternative)
usermod -U username   # Unlock account (alternative)

# Examples:
passwd -S john
chage -M 60 john
```

### File Security

```bash
# File integrity
md5sum file           # Calculate MD5 hash
sha256sum file        # Calculate SHA256 hash
sha512sum file        # Calculate SHA512 hash

# File permissions
chmod 600 file        # User read/write only
chmod 400 file        # User read only
chmod 700 directory   # User full access to directory

# Examples:
md5sum important_file
chmod 600 ~/.ssh/id_rsa
```

### Network Security

```bash
# Firewall management (iptables)
iptables -L           # List firewall rules
iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
iptables -A INPUT -j DROP                       # Drop other traffic

# Firewall management (ufw - Ubuntu)
ufw status            # Show firewall status
ufw enable            # Enable firewall
ufw allow 22          # Allow SSH
ufw deny 23           # Deny telnet

# Examples:
ufw enable
ufw allow 80/tcp
ufw allow 443/tcp
```

### Security Auditing

```bash
# System scanning
lynis audit system    # Security audit
rkhunter --check      # Rootkit detection
chkrootkit            # Rootkit detection (alternative)

# Log analysis
grep "Failed password" /var/log/auth.log  # Failed login attempts
grep "Invalid user" /var/log/auth.log     # Invalid user attempts

# Examples:
lynis audit system
grep "Failed password" /var/log/auth.log | wc -l
```

---

## Troubleshooting

### System Issues

```bash
# Boot issues
dmesg | grep -i error # Check for boot errors
journalctl -b         # Show boot logs
systemctl list-jobs   # Show pending jobs

# Disk issues
fsck /dev/sda1        # Check filesystem
badblocks /dev/sda    # Check for bad blocks
smartctl -a /dev/sda  # Check disk health

# Memory issues
free -h               # Check memory usage
vmstat 1 5            # Virtual memory stats
dmesg | grep -i "out of memory"  # Check for OOM

# Examples:
dmesg | grep -i error
fsck -y /dev/sda1
```

### Network Issues

```bash
# Network connectivity
ping -c 4 8.8.8.8     # Test basic connectivity
traceroute 8.8.8.8    # Check network path
nslookup google.com   # Test DNS resolution
dig google.com        # DNS lookup (detailed)

# Network configuration
ip route show         # Show routing table
ip addr show          # Show IP addresses
cat /etc/resolv.conf  # Show DNS configuration

# Examples:
ping -c 4 google.com
traceroute google.com
```

### Service Issues

```bash
# Service troubleshooting
systemctl status service    # Check service status
journalctl -u service      # Check service logs
systemctl restart service  # Restart service

# Port issues
netstat -tuln | grep :80   # Check if port 80 is listening
lsof -i :80                # Show what's using port 80
ss -tuln | grep :80        # Modern alternative

# Examples:
systemctl status nginx
journalctl -u nginx --since "1 hour ago"
```

---

## Performance Monitoring

### System Performance

```bash
# CPU monitoring
top                    # Real-time CPU usage
htop                   # Enhanced top
mpstat 1 5             # CPU statistics
iostat 1 5             # I/O statistics

# Memory monitoring
free -h                # Memory usage
vmstat 1 5             # Virtual memory stats
cat /proc/meminfo      # Detailed memory info

# Disk I/O monitoring
iotop                  # I/O by process
iostat -x 1 5          # Extended I/O stats
dd if=/dev/zero of=test bs=1M count=100  # Test disk speed

# Examples:
top -p $(pgrep nginx)
iostat -x 1 5
```

### Network Performance

```bash
# Network monitoring
iftop                  # Real-time network usage
nethogs                # Network usage by process
ss -i                  # Socket statistics
cat /proc/net/dev      # Network interface statistics

# Network testing
iperf3 -s              # Start iperf server
iperf3 -c server       # Test bandwidth to server
speedtest-cli          # Internet speed test

# Examples:
iftop -i eth0
iperf3 -c 192.168.1.100
```

### Application Performance

```bash
# Process profiling
strace -p PID          # Trace system calls
ltrace -p PID          # Trace library calls
perf top               # Performance analysis

# Memory profiling
valgrind --tool=memcheck ./program  # Memory leak detection
pmap PID               # Process memory map

# Examples:
strace -p $(pgrep nginx)
valgrind --tool=memcheck ./myapp
```

---

**Note**: This guide covers the most commonly used Linux and Unix commands. For more detailed information about specific commands, use `man command` or `command --help`. Always be careful with system administration commands, especially when using `sudo`.

### Shell Management

```bash
# Change login shell
chsh -s $(which zsh)           # Change to zsh
sudo chsh -s /bin/zsh userName # Change specific user's shell

# Alternative methods
sudo vim /etc/passwd           # Edit passwd file directly
# Update /bin/bash to /bin/zsh in the file

# Startup file for default shell
vim .Login                      # Edit login file

# Determine current shell
echo $0                         # Show current shell name
echo $SHELL                     # Show default shell

# Examples:
chsh -s $(which zsh)
echo $0                         # Output: zsh
```

### System Information

```bash
# Find Linux version
uname -r                        # Kernel version
cat /etc/os-release            # OS release information
lsb_release -a                 # LSB release information

# Examples:
uname -r                       # Output: 5.4.0-42-generic
cat /etc/os-release           # Shows detailed OS info
lsb_release -a                # Shows distribution info
```

### Configuration Files

```bash
# Vim configuration (.vimrc)
syntax on                      # Enable syntax highlighting
:set tabstop=4                # Set tab width to 4 spaces
:set shiftwidth=4             # Set indentation width
:set expandtab                # Use spaces instead of tabs

# Add to .vimrc file
echo "syntax on" >> ~/.vimrc
echo "set tabstop=4" >> ~/.vimrc
echo "set shiftwidth=4" >> ~/.vimrc
echo "set expandtab" >> ~/.vimrc
```

### Aliases and Shortcuts

```bash
# Create aliases
alias m="ssh -X yourSUNNetID@myth.stanford.edu"
alias ll="ls -la"
alias la="ls -A"
alias l="ls -CF"

# List all aliases
alias

# Remove alias
unalias alias_name

# Examples:
alias backup="rsync -avz /home/user/ /backup/"
alias update="sudo apt update && sudo apt upgrade"
```

### Advanced Search Commands

```bash
# Search subdirectories
echo **/*                    # List all sub-dirs and files
echo **/*(*)                 # List executable files
echo **/*(.)                 # List regular files

# Zsh-specific globbing
ls **/*.txt                  # Find all .txt files recursively
find . -name "*.txt"         # Alternative using find

# Examples:
echo **/*.py                 # List all Python files
echo **/*(/)                 # List only directories
```

