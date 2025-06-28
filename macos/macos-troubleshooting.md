# macOS Troubleshooting Guide

A practical guide to diagnosing and fixing common macOS issues for developers and power users.

---

## Table of Contents
1. [Startup & Boot Issues](#startup--boot-issues)
2. [Performance Problems](#performance-problems)
3. [Network & Connectivity](#network--connectivity)
4. [App Crashes & Freezes](#app-crashes--freezes)
5. [File & Disk Issues](#file--disk-issues)
6. [System Updates & Recovery](#system-updates--recovery)
7. [Security & Privacy](#security--privacy)
8. [Other Common Issues](#other-common-issues)
9. [Useful Tools & Resources](#useful-tools--resources)

---

## Startup & Boot Issues
- **Safe Mode**: Hold Shift during boot to load only essential system files
- **Reset NVRAM/PRAM**: Restart + Option + Command + P + R
- **Reset SMC**: Shut down, then hold Shift + Control + Option + Power for 10 seconds
- **Recovery Mode**: Command + R at boot for Disk Utility, reinstall, or restore
- **Verbose Mode**: Command + V at boot for detailed logs
- **Target Disk Mode**: T at boot to use Mac as external drive

## Performance Problems
- **Activity Monitor**: Check CPU, memory, disk, and energy usage
- **Free up RAM**: Close unused apps and browser tabs
- **Manage startup items**: System Preferences > Users & Groups > Login Items
- **Clear cache**: `~/Library/Caches/`
- **Check disk space**: Apple Menu > About This Mac > Storage
- **Reset Spotlight index**: System Preferences > Spotlight > Privacy (add/remove disk)
- **Update macOS and apps**

## Network & Connectivity
- **Wi-Fi diagnostics**: Hold Option and click Wi-Fi icon for details
- **Renew DHCP lease**: System Preferences > Network > Advanced > TCP/IP
- **Reset network settings**: Delete and re-add Wi-Fi in Network Preferences
- **Flush DNS cache**: `sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder`
- **Test with Ethernet**: Rule out Wi-Fi issues
- **Check router and ISP**

## App Crashes & Freezes
- **Force quit**: ⌘ + Option + Esc
- **Check for updates**: App Store or developer site
- **Delete app preferences**: `~/Library/Preferences/`
- **Reinstall app**
- **Check Console logs**: Applications > Utilities > Console

## File & Disk Issues
- **Disk Utility**: Use First Aid to repair disks
- **Check permissions**: Get Info > Sharing & Permissions
- **Recover deleted files**: Time Machine or recovery software
- **Unmounted drives**: Disk Utility > Mount
- **Corrupt files**: Try opening in another app or restoring from backup

## System Updates & Recovery
- **Update macOS**: System Preferences > Software Update
- **Reinstall macOS**: Command + R at boot
- **Restore from Time Machine**: Command + R > Restore from Time Machine
- **Create a bootable installer**: Use `createinstallmedia` tool

## Security & Privacy
- **Check for malware**: Use Malwarebytes or similar
- **Review privacy settings**: System Preferences > Security & Privacy
- **Enable Firewall**: System Preferences > Security & Privacy > Firewall
- **Check for unauthorized profiles**: System Preferences > Profiles
- **Reset passwords**: Apple ID or FileVault recovery

## Other Common Issues
- **Bluetooth not working**: Remove and re-add device, reset Bluetooth module
- **Audio issues**: Check output device, restart coreaudiod (`sudo killall coreaudiod`)
- **Printer problems**: Reset printing system (System Preferences > Printers & Scanners)
- **External displays**: Detect displays (System Preferences > Displays > Option-click Detect Displays)

## Useful Tools & Resources
- **Apple Support**: [support.apple.com](https://support.apple.com/)
- **MacRumors Forums**: [forums.macrumors.com](https://forums.macrumors.com/)
- **iFixit Guides**: [ifixit.com](https://www.ifixit.com/)
- **Malwarebytes**: [malwarebytes.com](https://www.malwarebytes.com/)
- **OnyX**: [titanium-software.fr](https://www.titanium-software.fr/en/onyx.html)

---

**Tip:** Regular backups and keeping your system updated are the best ways to prevent and recover from most macOS issues. 