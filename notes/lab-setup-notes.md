# Virtual Network Lab Setup Notes

## Objective
Set up a virtual networking lab environment using VirtualBox and Ubuntu Linux on macOS.

---

## Host Machine
- Device: MacBook Air
- Operating System: macOS

---

## Software Installed

### VirtualBox
Purpose:
Used to create and manage virtual machines.

### Ubuntu ISO
Purpose:
Used as the operating system for the Ubuntu virtual machine.

---

## Virtual Machine Created

### VM Name
Ubuntu

### Operating System
Ubuntu Linux

### Virtualization Platform
VirtualBox

---

## Tasks Completed
- Created project folder structure
- Created GitHub repository
- Installed VirtualBox
- Downloaded Ubuntu ISO
- Created Ubuntu virtual machine

---

## Issues Encountered
- Initial confusion locating project folders in Finder
- Learned how to access Home directory on macOS

---

## Skills Practiced
- Virtual machine setup
- File organization
- GitHub repository creation
- Basic virtualization concepts
- Documentation

---

## Next Steps
- Install Ubuntu inside VM
- Configure networking
- Learn Linux commands
- Install Wireshark
- Capture and analyze packets
## Evidence Captured
- GitHub repository created
- VirtualBox installed
- Ubuntu ISO downloaded
- Ubuntu VM created
- VS Code used for documentation
- Markdown preview successfully tested

## Current Status
Lab setup phase completed successfully. Ready to proceed to Windows 11 installation and network configuration.
## Issues Encountered

### Git Not Working on macOS
When attempting to run `git init`, the system returned an error indicating that no developer tools were installed.

### Cause
macOS requires Xcode Command Line Tools for Git and other development utilities to function.

### Solution
Installed Command Line Tools using:
```bash
xcode-select --install
```

A system popup appeared and the installation was completed successfully.

### Outcome
Git commands became available after installation, allowing the project setup to continue.
### GitHub Authentication Failure

While attempting to push the local repository to GitHub, password authentication failed.

### Cause
GitHub no longer supports account passwords for Git operations over HTTPS.

### Solution
Installed GitHub CLI and authenticated using browser-based login with:
```bash
gh auth login
```

### Outcome
Successfully authenticated Git and enabled repository push to GitHub.
## Git Push Issue - Remote Repository Conflict

### Problem
Push to GitHub was rejected with "fetch first" error.

### Cause
GitHub repository already contained initial commits (README/license), while local repository had separate commit history.

### Solution
Merged remote and local histories using:
```bash
git pull origin main --allow-unrelated-histories
```

Then pushed changes successfully.

### Outcome
Local project successfully synchronized with GitHub repository.