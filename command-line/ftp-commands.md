# FTP and SFTP Commands Guide

<p align="center">
  <img src="https://img.shields.io/badge/FTP-FF6600?style=flat-square&logo=ftp&logoColor=white" alt="FTP" />
  <img src="https://img.shields.io/badge/SFTP-3DDC84?style=flat-square&logo=ssh&logoColor=white" alt="SFTP" />
  <img src="https://img.shields.io/badge/SSH-000000?style=flat-square&logo=ssh&logoColor=white" alt="SSH" />
  <img src="https://img.shields.io/badge/File_Transfer-4A90E2?style=flat-square&logo=file-transfer&logoColor=white" alt="File Transfer" />
  <img src="https://img.shields.io/badge/Security-28A745?style=flat-square&logo=security&logoColor=white" alt="Security" />
  <img src="https://img.shields.io/badge/CLI-000000?style=flat-square&logo=terminal&logoColor=white" alt="CLI" />
</p>

A comprehensive guide to File Transfer Protocol (FTP) and Secure File Transfer Protocol (SFTP) commands for transferring files between systems.

---

## Table of Contents

1. [FTP Basics](#ftp-basics)
2. [FTP Commands](#ftp-commands)
3. [SFTP Commands](#sftp-commands)
4. [Command-Line FTP Clients](#command-line-ftp-clients)
5. [Automated File Transfers](#automated-file-transfers)
6. [Troubleshooting](#troubleshooting)

---

## FTP Basics

FTP (File Transfer Protocol) is a standard network protocol used for transferring files between a client and server. SFTP (SSH File Transfer Protocol) provides the same functionality over a secure SSH connection.

### Common FTP Ports
- **FTP**: Port 21 (control), Port 20 (data)
- **SFTP**: Port 22 (SSH)
- **FTPS**: Port 990 (implicit SSL), Port 21 (explicit SSL)

### Connection Types
- **Active Mode**: Server initiates data connection to client
- **Passive Mode**: Client initiates data connection to server (recommended for firewalls)

---

## FTP Commands

### Basic FTP Connection

```bash
# Connect to FTP server
ftp ftp.example.com
ftp 192.168.1.100
ftp -p ftp.example.com    # Passive mode
ftp -i ftp.example.com    # Interactive mode (no prompts)

# Connect with username
ftp username@ftp.example.com

# Examples:
ftp ftp.gnu.org
ftp -p ftp.debian.org
```

### FTP Client Commands

#### Navigation Commands

```bash
# Directory operations
pwd                     # Print working directory (remote)
lpwd                    # Print working directory (local)
cd directory            # Change remote directory
lcd directory           # Change local directory
mkdir directory         # Create remote directory
rmdir directory         # Remove remote directory
ls                      # List remote files
lls                     # List local files
dir                     # Detailed remote listing
```

#### File Transfer Commands

```bash
# Download files (get from server)
get filename            # Download single file
mget *.txt              # Download multiple files
get filename localname  # Download with different name

# Upload files (put to server)
put filename            # Upload single file
mput *.txt              # Upload multiple files
put filename remotename # Upload with different name

# Binary/ASCII mode
binary                  # Set binary transfer mode
ascii                   # Set ASCII transfer mode
```

#### Advanced Commands

```bash
# File operations
delete filename         # Delete remote file
mdelete *.tmp           # Delete multiple files
rename oldname newname  # Rename remote file

# Transfer options
prompt                  # Toggle interactive prompts
hash                    # Show transfer progress (#)
verbose                 # Toggle verbose mode
case                    # Toggle case sensitivity

# Connection
bye                     # Exit FTP
quit                    # Exit FTP
close                   # Close connection
open hostname           # Open new connection
```

#### FTP Session Examples

```bash
# Example 1: Download files
ftp ftp.example.com
cd /pub/files
ls
get important.txt
mget *.pdf
bye

# Example 2: Upload files
ftp ftp.example.com
cd /upload
put report.txt
mput *.log
ls
bye

# Example 3: Interactive session
ftp -i ftp.example.com
cd /downloads
prompt off
mget *.zip
hash
bye
```

---

## SFTP Commands

SFTP provides secure file transfer over SSH and uses similar commands to FTP.

### Basic SFTP Connection

```bash
# Connect to SFTP server
sftp user@hostname
sftp -P 2222 user@hostname    # Custom port
sftp -i keyfile user@hostname # Use specific key

# Examples:
sftp john@192.168.1.100
sftp -P 2222 admin@server.com
```

### SFTP Commands

#### Navigation Commands

```bash
# Directory operations
pwd                     # Print remote working directory
lpwd                    # Print local working directory
cd directory            # Change remote directory
lcd directory           # Change local directory
mkdir directory         # Create remote directory
rmdir directory         # Remove remote directory
ls                      # List remote files
lls                     # List local files
```

#### File Transfer Commands

```bash
# Download files
get filename            # Download single file
get filename localname  # Download with different name
mget *.txt              # Download multiple files
reget filename          # Resume interrupted download

# Upload files
put filename            # Upload single file
put filename remotename # Upload with different name
mput *.txt              # Upload multiple files
reput filename          # Resume interrupted upload

# Recursive transfers
get -r directory        # Download directory recursively
put -r directory        # Upload directory recursively
```

#### File Management Commands

```bash
# File operations
rm filename             # Delete remote file
rmdir directory         # Remove remote directory
rename oldname newname  # Rename remote file
chmod 644 filename      # Change file permissions
chown user filename     # Change file owner

# Local operations
lrm filename            # Delete local file
lmkdir directory        # Create local directory
```

#### Advanced Commands

```bash
# Options
progress                # Show transfer progress
recurse                 # Enable recursive mode
symlink                 # Create symbolic links

# Connection
bye                     # Exit SFTP
quit                    # Exit SFTP
exit                    # Exit SFTP
close                   # Close connection
open hostname           # Open new connection
```

#### SFTP Session Examples

```bash
# Example 1: Secure file download
sftp john@server.com
cd /home/john/documents
ls
get report.pdf
mget *.docx
bye

# Example 2: Secure file upload
sftp admin@backup-server.com
cd /backups
put important.zip
mput *.log
ls -la
bye

# Example 3: Recursive transfer
sftp user@host.com
get -r /var/logs
put -r ~/backup
bye
```

---

## Command-Line FTP Clients

### lftp (Advanced FTP Client)

```bash
# Install lftp
apt install lftp        # Debian/Ubuntu
yum install lftp        # CentOS/RHEL

# Basic usage
lftp ftp://ftp.example.com
lftp sftp://user@host.com

# Commands
ls                      # List files
get filename            # Download
put filename            # Upload
mirror directory        # Mirror directory
mirror -R directory     # Reverse mirror (upload)
```

### ncftp (Enhanced FTP Client)

```bash
# Install ncftp
apt install ncftp       # Debian/Ubuntu
yum install ncftp       # CentOS/RHEL

# Usage
ncftp ftp.example.com
ncftpget ftp.example.com localfile remotefile
ncftpput ftp.example.com remotedir localfile
```

### curl (HTTP/FTP Client)

```bash
# FTP with curl
curl -u username:password ftp://ftp.example.com/file.txt -o localfile.txt
curl -T localfile.txt ftp://ftp.example.com/ -u username:password

# SFTP with curl (requires libssh2)
curl -u username sftp://host.com/file.txt -o localfile.txt
```

---

## Automated File Transfers

### FTP Scripts

```bash
# Create FTP script
cat > ftp_script.txt << 'EOF'
open ftp.example.com
user username password
cd /upload
put file.txt
bye
EOF

# Execute script
ftp -n < ftp_script.txt
```

### SFTP Scripts

```bash
# Create SFTP script
cat > sftp_script.txt << 'EOF'
cd /backup
put important.txt
mput *.log
bye
EOF

# Execute script
sftp -b sftp_script.txt user@host.com
```

### Automated Transfers with expect

```bash
#!/usr/bin/expect
spawn ftp ftp.example.com
expect "Name:"
send "username\r"
expect "Password:"
send "password\r"
expect "ftp>"
send "cd /upload\r"
expect "ftp>"
send "put file.txt\r"
expect "ftp>"
send "bye\r"
expect eof
```

### Cron Jobs for Automated Transfers

```bash
# Add to crontab for daily backup
0 2 * * * /usr/bin/sftp -b /path/to/sftp_script.txt user@host.com

# Weekly file sync
0 3 * * 0 /usr/bin/rsync -avz /local/path/ user@host:/remote/path/
```

---

## Troubleshooting

### Common FTP Issues

```bash
# Connection problems
ftp: connect: Connection refused
# Solution: Check if FTP server is running and port is open

# Authentication issues
530 Login incorrect
# Solution: Verify username/password

# Passive mode issues
425 Can't build data connection
# Solution: Use passive mode (-p flag)

# File permission issues
550 Permission denied
# Solution: Check file permissions and ownership
```

### Debugging Commands

```bash
# Enable verbose mode
ftp -d ftp.example.com    # Debug mode
sftp -v user@host.com     # Verbose mode

# Test connectivity
telnet ftp.example.com 21
nc -zv ftp.example.com 21

# Check FTP server status
systemctl status vsftpd
systemctl status proftpd
```

### Security Best Practices

```bash
# Use SFTP instead of FTP when possible
sftp user@host.com

# Use key-based authentication
ssh-keygen -t rsa
ssh-copy-id user@host.com

# Disable anonymous FTP access
# Edit /etc/vsftpd.conf or /etc/proftpd/proftpd.conf

# Use firewall rules
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
iptables -A INPUT -p tcp --dport 20 -j ACCEPT
```

### Performance Optimization

```bash
# Use binary mode for non-text files
binary

# Enable compression if available
# Use lftp with compression
lftp -c "set ftp:ssl-allow no; open ftp://host; mirror directory"

# Use parallel transfers
# lftp supports parallel downloads
lftp -c "set sftp:auto-confirm yes; open sftp://host; mirror -P 5 directory"
```

---

## Quick Reference

### Essential FTP Commands
```bash
ftp hostname              # Connect
cd directory              # Change directory
ls                        # List files
get filename              # Download
put filename              # Upload
bye                       # Exit
```

### Essential SFTP Commands
```bash
sftp user@host            # Connect
cd directory              # Change directory
ls                        # List files
get filename              # Download
put filename              # Upload
bye                       # Exit
```

### Common Options
```bash
ftp -p hostname           # Passive mode
ftp -i hostname           # Interactive mode
sftp -P port user@host    # Custom port
sftp -i keyfile user@host # Use key file
```

This guide covers the essential FTP and SFTP commands needed for file transfer operations in command-line environments. 