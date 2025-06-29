# Linux Command Line Tutorial

A comprehensive guide to mastering the Linux command line interface, covering essential concepts, file management, text editing, user administration, permissions, process management, and software installation.

## Table of Contents

1. [Introduction to the Command Line](#introduction-to-the-command-line)
2. [File and Directory Management](#file-and-directory-management)
3. [Text File Creation and Editing](#text-file-creation-and-editing)
4. [User and Group Management](#user-and-group-management)
5. [File Access Control](#file-access-control)
6. [Process Monitoring and Management](#process-monitoring-and-management)
7. [Software Package Management](#software-package-management)

---

## Introduction to the Command Line

### Accessing the Command Line Interface

The command line interface (CLI) is a text-based interface for interacting with the Linux operating system. It provides direct access to system resources and allows for powerful automation and scripting capabilities.

#### Shell Prompt Indicators

- `$` indicates you are logged in as a normal user
- `#` indicates you are logged in as the administrator (root) user

#### Basic Command Structure

Commands entered at the shell prompt have three basic parts:

1. **Command**: The name of the program to run
2. **Options**: Adjust the behavior of the command (start with `-` or `--`)
3. **Arguments**: Targets that the command should operate upon

**Example**: `usermod -L morgan`
- Command: `usermod`
- Option: `-L`
- Argument: `morgan`

#### Essential Commands

```bash
# Display system hostname
$ hostname

# Show current date and time
$ date

# List files in /boot directory with details
$ ls -l /boot

# List all files including hidden ones
$ ls -all /boot
# or
$ ls --all /boot
```

#### Remote Access with SSH

```bash
# Connect to remote server
$ ssh servera.lab.example.com

# Connect as specific user
$ ssh root@servera.lab.example.com

# Generate SSH key pair
$ ssh-keygen

# Copy public key to remote machine
$ ssh-copy-id servera.lab.example.com
```

### The Bash Shell

Bash (Bourne Again Shell) is the default shell on most Linux systems. When used interactively, it displays a prompt when waiting for user input.

#### Shell Prompt Format

```bash
[username@hostname current_directory]$
```

**Examples**:
```bash
[student@host ~]$     # Regular user in home directory
[root@host ~]#        # Root user (note the # symbol)
```

#### Command Syntax

```bash
command                    # Basic command
command [option] ...       # Command with options
command [option]... [arguments]...  # Full syntax
```

**Example**:
```bash
$ ls -ld /boot            # List directory with long format
```

---

## File and Directory Management

### Understanding File Paths

#### Absolute vs Relative Paths

**Absolute Paths**:
- Begin with `/` (root directory)
- Specify the complete path from root
- No ambiguity about location
- Example: `/home/student/foo`

**Relative Paths**:
- Do not begin with `/`
- Relative to current working directory
- May use `..` to reference parent directory
- Example: `student` (relative to current location)

#### File System Characteristics

- **Case-sensitive**: `FileCase.txt` and `filecase.txt` are different files
- **Path length limit**: 4095 bytes total, 255 bytes per component
- **Character support**: UTF-8 encoded Unicode (except `/` and NUL)
- **Space handling**: Spaces are valid but require quoting in commands

### Navigation Commands

#### Current Directory and Listing

```bash
# Show current working directory
$ pwd

# List directory contents
$ ls

# List with details
$ ls -l

# List all files including hidden
$ ls -la

# List recursively (include subdirectories)
$ ls -R
```

#### Changing Directories

```bash
# Change to specific directory
$ cd /home/student/Documents

# Change to home directory
$ cd
# or
$ cd ~

# Change to parent directory
$ cd ..

# Change to previous directory
$ cd -

# Change to current directory (no effect)
$ cd .
```

### File Management Commands

#### Creating Files and Directories

```bash
# Create empty file or update timestamp
$ touch filename.txt

# Create single directory
$ mkdir directory_name

# Create nested directories
$ mkdir -p A/B/C/D
```

#### Locating Files by Name

```bash
# Find files by name pattern
$ find /path -name "pattern"

# Find files by name (case-insensitive)
$ find /path -iname "pattern"

# Find files by type
$ find /path -type f    # files only
$ find /path -type d    # directories only

# Find files by size
$ find /path -size +1M  # larger than 1MB
$ find /path -size -1M  # smaller than 1MB
```

#### Copying Files and Directories

```bash
# Copy file
$ cp source_file destination_file

# Copy file to directory
$ cp file.txt /path/to/directory/

# Copy directory recursively
$ cp -r source_directory destination_directory

# Preserve attributes
$ cp -p source_file destination_file
```

#### Moving and Renaming

```bash
# Move file
$ mv source_file destination_file

# Move file to directory
$ mv file.txt /path/to/directory/

# Rename file
$ mv old_name.txt new_name.txt

# Move directory
$ mv source_directory destination_directory
```

#### Removing Files and Directories

```bash
# Remove file
$ rm filename.txt

# Remove multiple files
$ rm file1.txt file2.txt file3.txt

# Remove directory (must be empty)
$ rmdir directory_name

# Remove directory and contents
$ rm -r directory_name

# Force removal (no confirmation)
$ rm -f filename.txt

# Interactive removal (confirm each file)
$ rm -i filename.txt
```

### File Name Expansion and Globbing

#### Basic Globbing Patterns

```bash
# Match any characters
$ ls *.txt

# Match single character
$ ls file?.txt

# Match character range
$ ls file[1-5].txt

# Match specific characters
$ ls file[abc].txt

# Match any number of characters
$ ls file*.txt
```

#### Tilde and Brace Expansion

```bash
# Tilde expansion (home directory)
$ cd ~
$ ls ~/Documents

# Brace expansion
$ echo file{1,2,3}.txt
# Output: file1.txt file2.txt file3.txt

$ echo file{1..5}.txt
# Output: file1.txt file2.txt file3.txt file4.txt file5.txt

$ mkdir -p /tmp/{dir1,dir2,dir3}
```

#### Command Substitution

```bash
# Execute command and use output
$ echo "Current date: $(date)"
$ echo "Current user: $(whoami)"

# Alternative syntax
$ echo "Current date: `date`"
```

#### Protecting Arguments from Expansion

```bash
# Single quotes (no expansion)
$ echo 'Current date: $(date)'
# Output: Current date: $(date)

# Double quotes (some expansion)
$ echo "Current date: $(date)"
# Output: Current date: Mon Feb 8 16:15:00 EST 2024

# Escape special characters
$ echo \$HOME
# Output: $HOME
```

---

## Text File Creation and Editing

### The Vim Text Editor

Vim is a powerful, modal text editor that is available on virtually all Linux systems. It's highly configurable and efficient for experienced users.

#### Vim Modes

1. **Command Mode**: Default mode for navigation and text manipulation
2. **Insert Mode**: For entering and editing text
3. **Visual Mode**: For selecting text
4. **Extended Command Mode**: For file operations and quitting

#### Basic Vim Workflow

**Opening a file**:
```bash
$ vim filename.txt
```

**Basic editing cycle**:
1. Use arrow keys to position cursor
2. Press `i` to enter insert mode
3. Type text
4. Press `Esc` to return to command mode
5. Use `u` to undo if needed

**Saving and exiting**:
```bash
:w          # Write (save) file and stay in command mode
:wq         # Write file and quit Vim
:q!         # Quit without saving changes
```

#### Text Manipulation Commands

**Deleting text**:
- `x` - Delete character under cursor
- `dd` - Delete entire line
- `dw` - Delete word

**Copy and paste (yank and put)**:
1. Position cursor at start of text
2. Press `v` to enter visual mode
3. Use arrow keys to select text
4. Press `y` to yank (copy)
5. Move cursor to destination
6. Press `p` to put (paste)

**Navigation**:
- `h`, `j`, `k`, `l` - Move left, down, up, right
- `0` - Beginning of line
- `$` - End of line
- `gg` - Beginning of file
- `G` - End of file
- `:number` - Go to specific line

#### Advanced Vim Features

**Search and replace**:
```bash
/search_term    # Search forward
?search_term    # Search backward
n               # Next match
N               # Previous match
:%s/old/new/g   # Replace all occurrences
```

**Split screen editing**:
```bash
:split          # Horizontal split
:vsplit         # Vertical split
Ctrl+w, arrow   # Navigate between splits
```

---

## User and Group Management

### Understanding Users and Groups

#### What is a User?

Every process on the system runs as a particular user, and every file is owned by a user. Access to files and directories is restricted based on user identity.

**User information commands**:
```bash
# Show current user information
$ id

# Show information for specific user
$ id username

# Show user associated with files
$ ls -l /tmp

# Show processes and their users
$ ps au
```

#### User ID (UID) System

Users are tracked internally by UID numbers, with names mapped in `/etc/passwd`:

**UID Ranges**:
- 0: Root user
- 1-999: System users
- 1000+: Regular users

**/etc/passwd format** (seven colon-separated fields):
```
username:password:UID:GID:GECOS:home_directory:shell
```

### Groups

Groups allow multiple users to share access to files and directories. Each user has a primary group and can belong to multiple supplementary groups.

**Group information**:
```bash
# Show current user's groups
$ groups

# Show group information
$ id -Gn

# Show group members
$ getent group groupname
```

### The Root User

The root user (UID 0) has unlimited privileges and can access any file or perform any operation on the system.

**Root user characteristics**:
- UID 0
- Home directory: `/root`
- Can access any file
- Can kill any process
- Can install software
- Can modify system configuration

### Switching Users and Running Commands as Root

#### Using `su` (Switch User)

```bash
# Switch to root user
$ su -

# Switch to specific user
$ su - username

# Switch user without changing environment
$ su username

# Exit and return to original user
$ exit
```

#### Using `sudo` (Superuser Do)

`sudo` allows authorized users to run commands as root without knowing the root password.

**Basic sudo usage**:
```bash
# Run command as root
$ sudo command

# Run command as specific user
$ sudo -u username command

# Open root shell
$ sudo -i

# Preserve environment
$ sudo -E command
```

**sudo configuration**:
- Configuration file: `/etc/sudoers`
- Use `visudo` to edit safely
- Users in `wheel` group can use sudo (on many systems)

### Managing Local Users

#### Creating Users

```bash
# Create user with default settings
$ sudo useradd username

# Create user with specific home directory
$ sudo useradd -d /home/username username

# Create user with specific shell
$ sudo useradd -s /bin/bash username

# Create user with specific UID
$ sudo useradd -u 1001 username
```

#### Setting Passwords

```bash
# Set password for user
$ sudo passwd username

# Force password change on next login
$ sudo chage -d 0 username
```

#### Modifying Users

```bash
# Change user's home directory
$ sudo usermod -d /new/home username

# Change user's shell
$ sudo usermod -s /bin/bash username

# Lock user account
$ sudo usermod -L username

# Unlock user account
$ sudo usermod -U username
```

#### Deleting Users

```bash
# Delete user (keep home directory)
$ sudo userdel username

# Delete user and home directory
$ sudo userdel -r username
```

### Managing Groups

#### Creating and Managing Groups

```bash
# Create group
$ sudo groupadd groupname

# Add user to group
$ sudo usermod -aG groupname username

# Remove user from group
$ sudo gpasswd -d username groupname

# Delete group
$ sudo groupdel groupname
```

#### Supplementary Groups

Users can belong to multiple groups beyond their primary group:

```bash
# Show user's groups
$ groups username

# Add user to supplementary group
$ sudo usermod -aG groupname username

# Show group members
$ getent group groupname
```

---

## File Access Control

### Linux File System Permissions

Linux uses a permission system to control access to files and directories. Permissions are displayed using a 10-character string.

#### Permission String Format

```
-rw-r--r-- 1 user group size date filename
```

**Character positions**:
1. File type (`-`, `d`, `l`, etc.)
2-4. Owner permissions (read, write, execute)
5-7. Group permissions (read, write, execute)
8-10. Other permissions (read, write, execute)

#### Permission Types

- **r (read)**: 4 - Can view file contents or list directory
- **w (write)**: 2 - Can modify file or create/delete in directory
- **x (execute)**: 1 - Can run file or access directory

#### Permission Examples

```bash
-rw-r--r--  # Regular file, owner can read/write, others can read
drwxr-xr-x  # Directory, owner can read/write/access, others can read/access
-rwx------  # Executable file, only owner can access
drwx------  # Directory, only owner can access
```

### Viewing Permissions and Ownership

```bash
# List files with permissions
$ ls -l

# List all files including hidden
$ ls -la

# Show permissions for specific file
$ ls -l filename

# Show ownership information
$ stat filename
```

### Changing File Permissions

#### Using Symbolic Mode

```bash
# Add execute permission for owner
$ chmod u+x filename

# Remove write permission for group
$ chmod g-w filename

# Set read/write for owner, read for others
$ chmod u=rw,o=r filename

# Add execute for all users
$ chmod +x filename
```

**Symbolic operators**:
- `+` - Add permission
- `-` - Remove permission
- `=` - Set exact permission

**Symbolic classes**:
- `u` - User (owner)
- `g` - Group
- `o` - Others
- `a` - All

#### Using Numeric Mode

```bash
# Set permissions using octal numbers
$ chmod 644 filename    # rw-r--r--
$ chmod 755 filename    # rwxr-xr-x
$ chmod 600 filename    # rw-------
$ chmod 750 filename    # rwxr-x---
```

**Common permission combinations**:
- `644` - Regular files (owner read/write, others read)
- `755` - Executable files and directories
- `600` - Private files (owner only)
- `750` - Group-shared files

#### Recursive Permission Changes

```bash
# Change permissions recursively
$ chmod -R 755 directory/

# Change permissions for files only
$ find directory/ -type f -exec chmod 644 {} \;

# Change permissions for directories only
$ find directory/ -type d -exec chmod 755 {} \;
```

### Changing Ownership

```bash
# Change file owner
$ sudo chown newowner filename

# Change file group
$ sudo chgrp newgroup filename

# Change both owner and group
$ sudo chown newowner:newgroup filename

# Change ownership recursively
$ sudo chown -R newowner:newgroup directory/
```

### Special Permissions

#### Set User ID (SUID)

When set on an executable file, the program runs with the owner's permissions:

```bash
# Set SUID bit
$ sudo chmod u+s filename

# Remove SUID bit
$ sudo chmod u-s filename

# Using numeric mode (4000)
$ sudo chmod 4755 filename
```

#### Set Group ID (SGID)

When set on a directory, new files inherit the group ownership:

```bash
# Set SGID bit on directory
$ sudo chmod g+s directory/

# Remove SGID bit
$ sudo chmod g-s directory/

# Using numeric mode (2000)
$ sudo chmod 2755 directory/
```

#### Sticky Bit

When set on a directory, only the file owner can delete files:

```bash
# Set sticky bit
$ sudo chmod +t directory/

# Remove sticky bit
$ sudo chmod -t directory/

# Using numeric mode (1000)
$ sudo chmod 1755 directory/
```

### Default File Permissions

#### Understanding umask

The `umask` command controls default permissions for new files and directories:

```bash
# Show current umask
$ umask

# Set umask
$ umask 022

# Calculate permissions
# Files: 666 - umask
# Directories: 777 - umask
```

**Common umask values**:
- `022` - Files: 644, Directories: 755
- `002` - Files: 664, Directories: 775
- `077` - Files: 600, Directories: 700

---

## Process Monitoring and Management

### Understanding Processes

A process is a running instance of a program. Each process has a unique Process ID (PID) and runs under a specific user account.

#### Process States

- **Running (R)**: Currently executing
- **Sleeping (S)**: Waiting for an event
- **Stopped (T)**: Suspended by a signal
- **Zombie (Z)**: Terminated but not cleaned up

### Listing Processes

#### Using `ps` Command

```bash
# Show processes in current shell
$ ps

# Show all processes with terminal
$ ps a

# Show all processes
$ ps aux

# Show processes in tree format
$ ps auxf

# Show specific user's processes
$ ps -u username
```

#### Using `top` Command

```bash
# Interactive process viewer
$ top

# Show specific number of processes
$ top -n 10

# Update every 2 seconds
$ top -d 2

# Show processes for specific user
$ top -u username
```

#### Using `htop` Command

```bash
# Enhanced interactive process viewer
$ htop

# Show specific number of processes
$ htop -p PID1,PID2
```

### Process Control

#### Background and Foreground Jobs

```bash
# Start process in background
$ command &

# List background jobs
$ jobs

# Bring job to foreground
$ fg %job_number

# Continue job in background
$ bg %job_number

# Suspend current process
$ Ctrl+Z
```

#### Process Signals

Signals are used to communicate with processes:

```bash
# Send signal to process
$ kill -signal PID

# Common signals
$ kill -TERM PID    # Terminate (default)
$ kill -KILL PID    # Force kill
$ kill -HUP PID     # Hangup (reload)
$ kill -STOP PID    # Stop process
$ kill -CONT PID    # Continue stopped process
```

**Signal numbers**:
- `1` - HUP (hangup)
- `2` - INT (interrupt)
- `9` - KILL (force kill)
- `15` - TERM (terminate)

#### Using `killall` and `pkill`

```bash
# Kill processes by name
$ killall process_name

# Kill processes by pattern
$ pkill pattern

# Force kill by name
$ killall -9 process_name
```

### System Monitoring

#### Load Average

Load average indicates system demand:

```bash
# Show load average
$ uptime

# Show load average with w
$ w

# Show detailed system info
$ cat /proc/loadavg
```

**Load average interpretation**:
- Values represent average system load over 1, 5, and 15 minutes
- For single CPU: 1.0 = 100% utilization
- For multi-CPU: divide by number of CPUs

#### Memory Usage

```bash
# Show memory usage
$ free

# Show memory in human-readable format
$ free -h

# Show detailed memory info
$ cat /proc/meminfo
```

#### Disk Usage

```bash
# Show disk usage
$ df

# Show disk usage in human-readable format
$ df -h

# Show directory sizes
$ du -sh directory/

# Show largest directories
$ du -sh /* | sort -hr
```

### Real-time Process Monitoring

#### Using `iotop`

```bash
# Monitor disk I/O by process
$ sudo iotop

# Show only processes doing I/O
$ sudo iotop -o
```

#### Using `nethogs`

```bash
# Monitor network usage by process
$ sudo nethogs

# Monitor specific interface
$ sudo nethogs interface_name
```

---

## Software Package Management

### Understanding Software Packages

Software packages contain programs, libraries, and configuration files needed to run applications. Package managers handle installation, updates, and removal of software.

#### Package Formats

- **RPM**: Red Hat Package Manager (Red Hat, CentOS, Fedora)
- **DEB**: Debian package format (Debian, Ubuntu)
- **TAR.GZ**: Source code archives

### The YUM Package Manager

YUM (Yellowdog Updater Modified) is a high-level package manager for RPM-based systems.

#### Basic YUM Commands

```bash
# Search for packages
$ yum search package_name

# Show package information
$ yum info package_name

# List available packages
$ yum list available

# List installed packages
$ yum list installed

# List updates available
$ yum list updates
```

#### Installing and Removing Software

```bash
# Install package
$ sudo yum install package_name

# Install multiple packages
$ sudo yum install package1 package2 package3

# Remove package
$ sudo yum remove package_name

# Remove package and dependencies
$ sudo yum autoremove package_name

# Update package
$ sudo yum update package_name

# Update all packages
$ sudo yum update
```

#### Working with Package Groups

```bash
# List available groups
$ yum group list

# Show group information
$ yum group info "group_name"

# Install group
$ sudo yum group install "group_name"

# Remove group
$ sudo yum group remove "group_name"
```

#### Package Management Features

```bash
# Check for updates
$ yum check-update

# Download package without installing
$ yumdownloader package_name

# Reinstall package
$ sudo yum reinstall package_name

# Downgrade package
$ sudo yum downgrade package_name
```

### Viewing Transaction History

```bash
# Show recent transactions
$ yum history

# Show specific transaction
$ yum history info transaction_id

# Undo transaction
$ sudo yum history undo transaction_id

# Redo transaction
$ sudo yum history redo transaction_id
```

### YUM Configuration

#### Repository Management

```bash
# List enabled repositories
$ yum repolist

# List all repositories
$ yum repolist all

# Enable repository
$ sudo yum-config-manager --enable repository_name

# Disable repository
$ sudo yum-config-manager --disable repository_name
```

#### YUM Configuration Files

- `/etc/yum.conf` - Main configuration file
- `/etc/yum.repos.d/` - Repository configuration files

### Alternative Package Managers

#### DNF (Dandified YUM)

DNF is the next-generation version of YUM:

```bash
# Install package
$ sudo dnf install package_name

# Update system
$ sudo dnf update

# Search packages
$ dnf search package_name
```

#### RPM Commands

Direct RPM manipulation:

```bash
# Install RPM package
$ sudo rpm -ivh package.rpm

# Upgrade RPM package
$ sudo rpm -Uvh package.rpm

# Query installed package
$ rpm -q package_name

# Show package files
$ rpm -ql package_name
```

### Best Practices

#### System Maintenance

```bash
# Regular system updates
$ sudo yum update

# Clean package cache
$ sudo yum clean all

# Check for security updates
$ sudo yum update --security

# Verify package integrity
$ sudo rpm -Va
```

#### Troubleshooting

```bash
# Check for broken dependencies
$ sudo yum check

# Rebuild package database
$ sudo yum clean all
$ sudo yum makecache

# Check package conflicts
$ sudo yum check-update
```

---

## Conclusion

This tutorial has covered the essential aspects of Linux command line administration, from basic navigation to advanced system management. Mastery of these concepts provides a solid foundation for Linux system administration and development work.

### Key Takeaways

1. **Command Line Efficiency**: The CLI provides powerful tools for system administration
2. **File System Understanding**: Proper file and permission management is crucial
3. **User Management**: Secure user and group administration is essential
4. **Process Control**: Understanding process management enables system optimization
5. **Package Management**: Proper software installation and maintenance ensures system stability

### Next Steps

- Practice these commands in a safe environment
- Explore advanced shell scripting
- Learn about system services and daemons
- Study network configuration and security
- Explore containerization and virtualization

### Additional Resources

- Linux man pages: `man command_name`
- GNU/Linux documentation
- System administration guides
- Online Linux communities and forums

---

*This tutorial provides a comprehensive introduction to Linux command line administration. For advanced topics and specific distributions, refer to official documentation and community resources.* 