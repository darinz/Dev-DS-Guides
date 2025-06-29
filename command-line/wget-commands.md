# wget Commands Guide

<p align="center">
  <img src="https://img.shields.io/badge/wget-6A5ACD?style=flat-square&logo=gnu&logoColor=white" alt="wget" />
  <img src="https://img.shields.io/badge/HTTP-FF6600?style=flat-square&logo=http&logoColor=white" alt="HTTP" />
  <img src="https://img.shields.io/badge/HTTPS-28A745?style=flat-square&logo=https&logoColor=white" alt="HTTPS" />
  <img src="https://img.shields.io/badge/FTP-FF6600?style=flat-square&logo=ftp&logoColor=white" alt="FTP" />
  <img src="https://img.shields.io/badge/Download-4A90E2?style=flat-square&logo=download&logoColor=white" alt="Download" />
  <img src="https://img.shields.io/badge/CLI-000000?style=flat-square&logo=terminal&logoColor=white" alt="CLI" />
</p>

A comprehensive guide to using wget (GNU Wget) for downloading files from the web, including HTTP, HTTPS, and FTP protocols.

---

## Table of Contents

1. [wget Basics](#wget-basics)
2. [Basic Download Commands](#basic-download-commands)
3. [Advanced Download Options](#advanced-download-options)
4. [Recursive Downloads](#recursive-downloads)
5. [Authentication and Security](#authentication-and-security)
6. [Download Management](#download-management)
7. [Scripting and Automation](#scripting-and-automation)
8. [Troubleshooting](#troubleshooting)

---

## wget Basics

wget is a free utility for non-interactive downloading of files from the web. It supports HTTP, HTTPS, and FTP protocols, and can retrieve files through HTTP proxies.

### Key Features
- **Resume interrupted downloads**
- **Recursive downloading**
- **Background operation**
- **Authentication support**
- **Proxy support**
- **Rate limiting**
- **Mirror websites**

### Installation

```bash
# Debian/Ubuntu
apt install wget

# CentOS/RHEL
yum install wget

# macOS
brew install wget

# Check version
wget --version
```

---

## Basic Download Commands

### Simple Downloads

```bash
# Download a single file
wget https://example.com/file.zip

# Download with custom filename
wget -O custom_name.zip https://example.com/file.zip

# Download to specific directory
wget -P /path/to/directory https://example.com/file.zip

# Examples:
wget https://ftp.gnu.org/gnu/wget/wget-1.21.3.tar.gz
wget -O ubuntu.iso https://releases.ubuntu.com/22.04/ubuntu-22.04.3-desktop-amd64.iso
wget -P ~/Downloads https://example.com/document.pdf
```

### Download Progress and Output

```bash
# Show progress bar
wget https://example.com/file.zip

# Quiet mode (no output)
wget -q https://example.com/file.zip

# Verbose output
wget -v https://example.com/file.zip

# Show progress without other output
wget -q --show-progress https://example.com/file.zip

# Examples:
wget -q --show-progress https://example.com/large-file.iso
wget -v https://example.com/file.txt
```

### Multiple File Downloads

```bash
# Download multiple files
wget https://example.com/file1.txt https://example.com/file2.txt

# Download from a list file
wget -i file_list.txt

# Create file list
cat > downloads.txt << 'EOF'
https://example.com/file1.pdf
https://example.com/file2.zip
https://example.com/file3.txt
EOF

# Download from list
wget -i downloads.txt

# Examples:
wget https://ftp.gnu.org/gnu/bash/bash-5.2.tar.gz \
     https://ftp.gnu.org/gnu/coreutils/coreutils-9.3.tar.gz
```

---

## Advanced Download Options

### Resume and Continue Downloads

```bash
# Continue interrupted download
wget -c https://example.com/file.zip

# Continue with timestamp checking
wget -N https://example.com/file.zip

# Examples:
wget -c https://example.com/large-file.iso
wget -N https://example.com/updated-file.txt
```

### Rate Limiting and Bandwidth Control

```bash
# Limit download speed (bytes per second)
wget --limit-rate=1m https://example.com/file.zip

# Limit download speed (kilobytes per second)
wget --limit-rate=512k https://example.com/file.zip

# Wait between requests
wget --wait=2 https://example.com/file.zip

# Random wait time
wget --random-wait https://example.com/file.zip

# Examples:
wget --limit-rate=2m https://example.com/large-file.iso
wget --wait=5 --random-wait https://example.com/file.txt
```

### Timeout and Retry Options

```bash
# Set timeout for connections
wget --timeout=30 https://example.com/file.zip

# Set timeout for reads
wget --read-timeout=60 https://example.com/file.zip

# Number of retries
wget --tries=3 https://example.com/file.zip

# Infinite retries
wget --tries=0 https://example.com/file.zip

# Examples:
wget --timeout=60 --tries=5 https://example.com/file.zip
wget --read-timeout=120 https://example.com/slow-file.zip
```

### User Agent and Headers

```bash
# Set custom user agent
wget --user-agent="Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36" https://example.com/file.zip

# Add custom headers
wget --header="Accept: text/html" https://example.com/file.zip

# Multiple headers
wget --header="Accept: application/json" \
     --header="Authorization: Bearer token123" \
     https://example.com/api/data

# Examples:
wget --user-agent="wget/1.21.3" https://example.com/file.zip
wget --header="Cookie: session=abc123" https://example.com/file.zip
```

---

## Recursive Downloads

### Basic Recursive Download

```bash
# Download a directory recursively
wget -r https://example.com/directory/

# Limit recursion depth
wget -r -l 2 https://example.com/directory/

# Download only specific file types
wget -r -A "*.pdf,*.txt" https://example.com/directory/

# Exclude specific file types
wget -r -R "*.tmp,*.log" https://example.com/directory/

# Examples:
wget -r -l 3 https://ftp.gnu.org/gnu/
wget -r -A "*.html,*.css,*.js" https://example.com/website/
```

### Mirror Websites

```bash
# Mirror a website
wget --mirror https://example.com/

# Mirror with timestamp checking
wget --mirror --timestamping https://example.com/

# Mirror with conversion
wget --mirror --convert-links https://example.com/

# Complete mirror
wget --mirror --convert-links --adjust-extension --page-requisites https://example.com/

# Examples:
wget --mirror --convert-links https://docs.example.com/
wget --mirror --timestamping --no-parent https://example.com/docs/
```

### Advanced Recursive Options

```bash
# Follow FTP directories
wget -r ftp://ftp.example.com/

# Don't follow parent directories
wget -r --no-parent https://example.com/directory/

# Follow only certain links
wget -r --accept-regex=".*\.pdf$" https://example.com/

# Reject certain links
wget -r --reject-regex=".*\.tmp$" https://example.com/

# Examples:
wget -r --no-parent https://example.com/downloads/
wget -r --accept-regex=".*\.(jpg|jpeg|png|gif)$" https://example.com/images/
```

---

## Authentication and Security

### HTTP Authentication

```bash
# Basic authentication
wget --user=username --password=password https://example.com/file.zip

# Prompt for password
wget --user=username --ask-password https://example.com/file.zip

# Use .netrc file
wget --netrc https://example.com/file.zip

# Examples:
wget --user=john --password=mypass https://example.com/private/file.zip
wget --user=admin --ask-password https://example.com/admin/file.zip
```

### FTP Authentication

```bash
# FTP with authentication
wget --ftp-user=username --ftp-password=password ftp://ftp.example.com/file.zip

# Anonymous FTP
wget ftp://ftp.gnu.org/gnu/wget/wget-1.21.3.tar.gz

# Examples:
wget --ftp-user=user --ftp-password=pass ftp://ftp.example.com/file.zip
wget ftp://ftp.debian.org/debian/dists/stable/main/binary-amd64/Packages.gz
```

### SSL/TLS Options

```bash
# Ignore SSL certificate errors
wget --no-check-certificate https://example.com/file.zip

# Use specific SSL protocol
wget --secure-protocol=TLSv1_2 https://example.com/file.zip

# Specify CA bundle
wget --ca-certificate=/path/to/ca-bundle.crt https://example.com/file.zip

# Examples:
wget --no-check-certificate https://self-signed.example.com/file.zip
wget --secure-protocol=TLSv1_3 https://example.com/file.zip
```

### Proxy Support

```bash
# HTTP proxy
wget --proxy=on --http-proxy=http://proxy.example.com:8080 https://example.com/file.zip

# HTTPS proxy
wget --proxy=on --https-proxy=https://proxy.example.com:8443 https://example.com/file.zip

# Proxy with authentication
wget --proxy=on --proxy-user=user --proxy-password=pass https://example.com/file.zip

# Examples:
wget --proxy=on --http-proxy=http://192.168.1.100:3128 https://example.com/file.zip
wget --proxy=on --proxy-user=john --proxy-password=secret https://example.com/file.zip
```

---

## Download Management

### Output and Logging

```bash
# Log to file
wget -o wget.log https://example.com/file.zip

# Append to log file
wget -a wget.log https://example.com/file.zip

# Log to both file and console
wget -o wget.log -v https://example.com/file.zip

# Examples:
wget -o download.log https://example.com/file.zip
wget -a wget.log --show-progress https://example.com/file.zip
```

### Background Downloads

```bash
# Download in background
wget -b https://example.com/file.zip

# Background with log file
wget -b -o wget.log https://example.com/file.zip

# Check background downloads
tail -f wget.log

# Examples:
wget -b -o download.log https://example.com/large-file.iso
wget -b -q -o wget.log https://example.com/file.zip
```

### Directory Structure

```bash
# Create directory structure
wget -r --no-host-directories https://example.com/directory/

# Use host directories
wget -r --host-directories https://example.com/directory/

# Custom directory structure
wget -r --cut-dirs=2 https://example.com/path/to/directory/

# Examples:
wget -r --no-host-directories https://example.com/docs/
wget -r --cut-dirs=1 https://example.com/downloads/software/
```

---

## Scripting and Automation

### Download Scripts

```bash
#!/bin/bash
# Download script example

# Create download directory
mkdir -p downloads

# Download files
wget -P downloads/ \
     --limit-rate=2m \
     --tries=3 \
     --timeout=60 \
     https://example.com/file1.zip \
     https://example.com/file2.zip

# Check for errors
if [ $? -eq 0 ]; then
    echo "Downloads completed successfully"
else
    echo "Some downloads failed"
fi
```

### Automated Downloads with Cron

```bash
# Add to crontab for daily downloads
0 2 * * * /usr/bin/wget -q -P /backup/ https://example.com/daily-backup.zip

# Weekly mirror update
0 3 * * 0 /usr/bin/wget --mirror --timestamping -P /mirror/ https://example.com/

# Examples:
# Download daily logs
0 1 * * * wget -q -P /var/log/backup/ https://example.com/logs/daily.log

# Mirror documentation weekly
0 4 * * 0 wget --mirror --convert-links -P /docs/ https://docs.example.com/
```

### Batch Downloads

```bash
#!/bin/bash
# Batch download script

# Read URLs from file
while IFS= read -r url; do
    echo "Downloading: $url"
    wget -q --show-progress "$url"
    if [ $? -eq 0 ]; then
        echo "Success: $url"
    else
        echo "Failed: $url"
    fi
done < urls.txt
```

### Download with Conditions

```bash
#!/bin/bash
# Conditional download script

url="https://example.com/file.zip"
local_file="file.zip"

# Download only if local file doesn't exist or is older
if [ ! -f "$local_file" ] || [ "$(curl -s -I "$url" | grep -i last-modified | cut -d' ' -f2-)" != "$(stat -c %y "$local_file" 2>/dev/null)" ]; then
    echo "Downloading updated file..."
    wget -O "$local_file" "$url"
else
    echo "File is up to date"
fi
```

---

## Troubleshooting

### Common Issues

```bash
# Connection refused
wget: unable to resolve host address
# Solution: Check DNS or use IP address

# Permission denied
wget: cannot write to 'file.zip' (Permission denied)
# Solution: Check directory permissions

# SSL certificate errors
wget: error getting response: SSL connect error
# Solution: Use --no-check-certificate or update CA certificates

# Timeout errors
wget: error getting response: Connection timed out
# Solution: Increase timeout values or check network
```

### Debugging Commands

```bash
# Enable debug output
wget -d https://example.com/file.zip

# Show server response
wget -S https://example.com/file.zip

# Test connection without downloading
wget --spider https://example.com/file.zip

# Check if file exists
wget --spider --server-response https://example.com/file.zip

# Examples:
wget -d -v https://example.com/file.zip
wget --spider https://example.com/file.zip && echo "File exists"
```

### Performance Optimization

```bash
# Use multiple connections (not supported in wget)
# Use aria2c instead for parallel downloads
aria2c -x 16 https://example.com/file.zip

# Optimize for large files
wget --limit-rate=5m --tries=3 --timeout=60 https://example.com/large-file.iso

# Use compression
wget --header="Accept-Encoding: gzip,deflate" https://example.com/file.txt

# Examples:
wget --limit-rate=10m --tries=5 https://example.com/ubuntu.iso
wget --header="Accept-Encoding: gzip" https://example.com/api/data.json
```

### Alternative Tools

```bash
# curl for single file downloads
curl -O https://example.com/file.zip

# aria2c for parallel downloads
aria2c -x 16 -s 16 https://example.com/file.zip

# rsync for synchronization
rsync -avz user@host:/path/ /local/path/

# Examples:
curl -L -O https://example.com/file.zip
aria2c -x 8 https://example.com/large-file.iso
```

---

## Quick Reference

### Essential wget Commands
```bash
wget url                    # Download file
wget -c url                 # Continue download
wget -O filename url        # Download with custom name
wget -P directory url       # Download to directory
wget -r url                 # Recursive download
wget --mirror url           # Mirror website
```

### Common Options
```bash
-q                          # Quiet mode
-v                          # Verbose mode
-c                          # Continue download
-O filename                 # Output filename
-P directory                # Output directory
-r                          # Recursive
-l depth                    # Recursion depth
--limit-rate=rate          # Limit speed
--tries=number             # Number of retries
--timeout=seconds          # Timeout
--user-agent=string        # User agent
--user=user --password=pass # Authentication
```

### Useful Combinations
```bash
# Download with progress and resume
wget -c --show-progress https://example.com/file.zip

# Mirror with conversion
wget --mirror --convert-links --adjust-extension https://example.com/

# Download with authentication and rate limiting
wget --user=user --password=pass --limit-rate=1m https://example.com/file.zip

# Recursive download with file type filtering
wget -r -A "*.pdf,*.txt" --no-parent https://example.com/directory/
```

This guide covers the essential wget commands needed for downloading files from the web in command-line environments. 