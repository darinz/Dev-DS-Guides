# Linux and Unix Command Line Reference

A concise reference of useful and common command line commands for Linux and Unix systems.

---

## Table of Contents

1. [File and Directory Operations](#file-and-directory-operations)
2. [File Viewing and Manipulation](#file-viewing-and-manipulation)
3. [User and Permission Management](#user-and-permission-management)
4. [Networking](#networking)
5. [Package Management](#package-management)
6. [Process Management](#process-management)
7. [Disk Usage and System Monitoring](#disk-usage-and-system-monitoring)
8. [Searching and Filtering](#searching-and-filtering)
9. [Archiving and Compression](#archiving-and-compression)
10. [Scripting and Miscellaneous](#scripting-and-miscellaneous)

---

## File and Directory Operations

```bash
ls           # List directory contents
# Example: ls -l /home/user
cd           # Change directory
# Example: cd /var/log
pwd          # Print working directory
# Example: pwd
mkdir        # Make directory
# Example: mkdir new_folder
rmdir        # Remove empty directory
# Example: rmdir old_folder
rm -r        # Remove directory and contents
# Example: rm -r unwanted_folder
cp -r        # Copy directory recursively
# Example: cp -r src_folder/ backup_folder/
mv           # Move or rename files/directories
# Example: mv file.txt /tmp/
rm           # Remove files
# Example: rm old_file.txt
touch <file> # Create an empty file or update file's timestamp
# Example: touch newfile.txt
```

## File Viewing and Manipulation

```bash
cat          # Concatenate and display file contents
# Example: cat notes.txt
less         # View file contents page-by-page
# Example: less bigfile.log
more         # View file contents page-by-page (less preferred)
# Example: more bigfile.log
tail -f      # Continuously view end of file (e.g., logs)
# Example: tail -f /var/log/syslog
head         # View beginning of file
# Example: head -n 10 example.txt
nano         # Simple text editor
# Example: nano script.sh
vim          # Advanced text editor
# Example: vim config.conf
```

## User and Permission Management

```bash
whoami       # Show current user
# Example: whoami
id           # Show user and group IDs
# Example: id
adduser      # Add a new user
# Example: sudo adduser john
passwd       # Change user password
# Example: passwd john
chmod        # Change file permissions
# Example: chmod 755 script.sh
chown        # Change file owner
# Example: chown user:group file.txt
groups       # Show group memberships
# Example: groups
```

## Networking

```bash
ping         # Test network connection
# Example: ping google.com
ifconfig     # Show network interfaces (deprecated)
# Example: ifconfig
ip addr      # Show IP addresses (preferred)
# Example: ip addr show
netstat -tuln # Show listening ports
# Example: netstat -tuln
ss -tuln     # Alternative to netstat
# Example: ss -tuln
curl         # Transfer data from or to a server
# Example: curl https://example.com
wget         # Download files from the web
# Example: wget https://example.com/file.zip
scp          # Secure copy between systems
# Example: scp file.txt user@192.168.1.10:/home/user/
ssh          # Secure shell connection
# Example: ssh user@remotehost
```

## Package Management

**Debian/Ubuntu**:

```bash
apt update              # Refresh package index
# Example: sudo apt update
apt upgrade             # Upgrade installed packages
# Example: sudo apt upgrade
apt install <package>   # Install package
# Example: sudo apt install git
apt remove <package>    # Remove package
# Example: sudo apt remove git
```

**RedHat/CentOS**:

```bash
yum install <package>   # Install package
# Example: sudo yum install nano
yum remove <package>    # Remove package
# Example: sudo yum remove nano
dnf install <package>   # Newer alternative to yum
# Example: sudo dnf install htop
```

## Process Management

```bash
ps aux        # Show running processes
# Example: ps aux | grep apache
top           # Interactive process viewer
# Example: top
htop          # Enhanced top (if installed)
# Example: htop
kill <pid>    # Kill process by PID
# Example: kill 1234
killall <name> # Kill all processes by name
# Example: killall firefox
&             # Run process in background
# Example: ./long_task.sh &
fg            # Bring process to foreground
# Example: fg %1
bg            # Resume background process
# Example: bg %1
jobs          # List current jobs
# Example: jobs
```

## Disk Usage and System Monitoring

```bash
df -h         # Disk space usage
# Example: df -h
du -sh <dir>  # Directory size
# Example: du -sh /home/user/
free -h       # Memory usage
# Example: free -h
uptime        # System uptime
# Example: uptime
uname -a      # System information
# Example: uname -a
dmesg         # Boot and system logs
# Example: dmesg | less
```

## Searching and Filtering

```bash
grep <pattern> <file>     # Search for pattern in file
# Example: grep 'error' logfile.txt
find <path> -name <file>  # Find file by name
# Example: find /home -name '*.jpg'
locate <file>             # Locate file in system (needs updatedb)
# Example: locate mydoc.pdf
updatedb                  # Update locate DB
# Example: sudo updatedb
awk '{print $1}'          # Pattern scanning and processing
# Example: awk '{print $1}' file.txt
sed 's/foo/bar/'          # Stream editor
# Example: sed 's/old/new/g' file.txt
```

## Archiving and Compression

```bash
tar -cvf file.tar dir     # Create tar archive
# Example: tar -cvf archive.tar mydir/
tar -xvf file.tar         # Extract tar archive
# Example: tar -xvf archive.tar
tar -czvf file.tar.gz dir # Create gzipped tar archive
# Example: tar -czvf archive.tar.gz mydir/
tar -xzvf file.tar.gz     # Extract gzipped tar archive
# Example: tar -xzvf archive.tar.gz
gzip file                 # Compress file
# Example: gzip logfile.txt
gunzip file.gz            # Decompress file
# Example: gunzip logfile.txt.gz
zip file.zip file         # Create zip archive
# Example: zip archive.zip file.txt
unzip file.zip            # Extract zip archive
# Example: unzip archive.zip
```

## Scripting and Miscellaneous

```bash
alias ll='ls -lah'        # Create alias
# Example: alias gs='git status'
history                   # Show command history
# Example: history
!!                        # Repeat last command
# Example: sudo !!
!n                        # Repeat nth command
# Example: !42
crontab -e                # Edit cron jobs
# Example: crontab -e
man <command>             # Show manual page
# Example: man ls
echo $VAR                 # Show value of VAR
# Example: echo $HOME
export VAR=value          # Set environment variable
# Example: export PATH=$PATH:/new/path
```

---

**Note**: This is not exhaustive but intended as a quick reference for commonly used commands.

