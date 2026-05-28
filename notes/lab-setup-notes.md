# Virtual Network Lab Setup Notes

```md
This project documents the creation of a multi-VM virtual networking lab using VirtualBox on macOS.

The lab was designed to simulate a small enterprise-style environment for practicing:
- Linux administration
- Windows administration
- Network troubleshooting
- SSH remote access
- Packet analysis with Wireshark
- Virtual networking concepts

The environment includes:
- Ubuntu Linux VM
- Windows 11 VM
- NAT internet connectivity
- Internal network communication
- Static IP configuration
- ICMP testing
- SSH remote administration
```

## Objective

Build a small virtual networking lab using VirtualBox, Ubuntu Linux, and Windows 11 on macOS to practice:

* Virtualization
* Network configuration
* IP addressing
* Connectivity troubleshooting
* Packet analysis
* Documentation
* GitHub project management

---

## Host Machine

| Component        | Details     |
| ---------------- | ----------- |
| Device           | MacBook Air |
| Operating System | macOS       |

---

## Software Installed

### Oracle VM VirtualBox

Purpose:
Used to create and manage virtual machines for the networking lab.

### Ubuntu ISO

Purpose:
Used to install Ubuntu Linux virtual machine.

### Windows 11 ISO

Purpose:
Used to install Windows 11 virtual machine.

### Wireshark

Purpose:
Used for packet capture and network traffic analysis.

### Visual Studio Code

Purpose:
Used for documentation and project organization.

### Git & GitHub CLI

Purpose:
Used for version control and GitHub repository management.

---

## Project Folder Structure

```text id="3jlwmx"
IT-Labs/
└── Virtual-Network-Lab/
    ├── screenshots/
    ├── notes/
    ├── configs/
    └── captures/
```

---

## Ubuntu Virtual Machine Setup

### VM Created

| Setting  | Value        |
| -------- | ------------ |
| Name     | Ubuntu       |
| Platform | VirtualBox   |
| OS Type  | Ubuntu Linux |

### Configuration

| Resource | Allocation  |
| -------- | ----------- |
| RAM      | 8 GB        |
| CPU      | 4 cores     |
| Storage  | Dynamic VDI |

### Installation Process

* Downloaded Ubuntu ISO
* Created Ubuntu VM in VirtualBox
* Mounted Ubuntu ISO
* Installed Ubuntu Linux
* Completed initial configuration

### Skills Practiced

* Virtual machine creation
* Linux installation
* Virtual storage configuration
* Resource allocation

---

## Windows 11 Virtual Machine Setup

### VM Created

| Setting  | Value             |
| -------- | ----------------- |
| Name     | Windows 11        |
| Platform | VirtualBox        |
| OS Type  | Microsoft Windows |

### Configuration

| Resource | Allocation        |
| -------- | ----------------- |
| RAM      | 8 GB              |
| CPU      | 4 cores           |
| Storage  | 70 GB Dynamic VDI |

### Installation Process

* Mounted Windows ISO
* Booted VM
* Installed Windows 11
* Completed initial Windows setup

### Purpose

Used to simulate a Windows workstation environment for networking and IT support practice.

### Skills Practiced

* Virtual machine deployment
* Windows installation
* Multi-OS virtualization
* System configuration

---

## Git and GitHub Setup

## Git Installation Issue

### Problem

Git commands failed on macOS because developer tools were missing.

### Cause

macOS requires Xcode Command Line Tools for Git functionality.

### Solution

Installed developer tools using:
xcode-select --install

### Result

Git commands became available successfully.

---

## GitHub Authentication Issue

### Problem

GitHub password authentication failed during repository push.

### Cause

GitHub no longer supports password authentication over HTTPS.

### Solution

Installed GitHub CLI and authenticated using:

```bash id="lwp0z4"
gh auth login
```

### Result

Successfully authenticated GitHub access.

---

## Git Push Conflict Issue

### Problem

Git push was rejected with:

```text id="lt8a3u"
fetch first
```

### Cause

The remote repository already contained commits while the local repository had separate commit history.

### Solution

Merged histories using:

```bash id="s8v4zd"
git pull origin main --allow-unrelated-histories
```

Then pushed changes successfully.

### Result

Local and remote repositories synchronized successfully.

---

## Networking Configuration

### Initial Network Problem

### Symptoms

* Windows received APIPA address (`169.254.x.x`)
* Ubuntu failed to obtain IPv4 address
* No VM-to-VM communication possible

### Cause

Incorrect VirtualBox network configuration and missing DHCP functionality on internal network.

---

## Final Network Architecture

### Adapter Configuration

Both virtual machines were configured with two network adapters in VirtualBox:

| Adapter   | Network Type                | Purpose                |
| --------- | --------------------------- | ---------------------- |
| Adapter 1 | NAT                         | Internet connectivity  |
| Adapter 2 | Internal Network (`labnet`) | VM-to-VM communication |

---

### Windows Network Configuration

| Interface  | Purpose          | IP Address       |
| ---------- | ---------------- | ---------------- |
| Ethernet   | NAT              | DHCP (10.0.2.15)  |
| Ethernet 2 | Internal Network | 192.168.56.10/24 |

---

### Ubuntu Network Configuration

| Interface | Purpose          | IP Address       |
| --------- | ---------------- | ---------------- |
| enp0s8    | NAT              | DHCP (10.0.2.15)  |
| enp0s9    | Internal Network | 192.168.56.20/24 |

---

## DHCP Troubleshooting Process

### Ubuntu NAT Failure

### Symptoms

* `ping 8.8.8.8` failed
* `Network is unreachable`
* No `10.0.2.x` address assigned
* No default route available
* NetworkManager continuously displayed:

```text id="j1g3m9"
getting IP configuration
```

### Diagnostic Commands Used

#### Interface Information

```bash id="ffmx6d"
ip addr
```

#### Routing Table

```bash id="xwq4n7"
ip route
```

#### NetworkManager Status

```bash id="apn6e8"
nmcli device status
```

#### Connection Information

```bash id="b3dy2s"
nmcli con show
```

---

## Root Cause Analysis

The primary issue was:

* incorrect interface mapping between:

  * `enp0s8`
  * `enp0s9`

A static internal-network configuration was mistakenly applied to the NAT interface.

This prevented DHCP from assigning an internet-access IPv4 address.

---

## DHCP Resolution

### Actions Taken

* Removed broken NetworkManager connection profiles
* Recreated NAT connection using `nmcli`
* Re-enabled DHCP on correct interface
* Restarted networking services
* Verified VirtualBox adapter assignments

### Result

Ubuntu successfully received:

* DHCP IPv4 address
* Default gateway
* Internet connectivity

---

## Internal Network Static IP Configuration

### Problem

VirtualBox Internal Network mode does not provide DHCP services automatically.

### Result

* Windows generated APIPA address (`169.254.103.109`)
* Ubuntu internal adapter had no IPv4 address

---

## Static IP Solution

Manual IPv4 addresses were configured:

| VM      | IP Address    |
| ------- | ------------- |
| Windows | 192.168.56.10 |
| Ubuntu  | 192.168.56.20 |

---

## Connectivity Testing

### Ubuntu → Windows

```bash id="4hxtqa"
ping 192.168.56.10
```

### Windows → Ubuntu

```cmd id="6jep0g"
ping 192.168.56.20
```

### Result

Successful ICMP communication between both virtual machines.

---

## Windows Firewall Issue

### Problem

Ubuntu could not initially ping Windows successfully.

### Cause

Windows Firewall blocked ICMP echo requests.

### Solution

Allowed ICMP traffic using:

```powershell id="7nq8zu"
netsh advfirewall firewall add rule name="ICMP Allow" protocol=icmpv4:8,any dir=in action=allow
```

### Result

Ping communication succeeded successfully.

---

## Packet Analysis with Wireshark

### Objective

Capture and analyze ICMP traffic between Ubuntu and Windows virtual machines.

### Tasks Performed

* Started packet capture
* Generated ping traffic
* Observed ICMP packets
* Verified packet exchange between VMs

### Skills Practiced

* Packet capture
* Protocol analysis
* ICMP troubleshooting
* Network traffic inspection

---

## Evidence Captured
```md
## Screenshots Captured

* virtualbox_dashboard.png
* ubuntu_ip_addr.png
* windows_ipconfig.png
* ubuntu_ping_windows.png
* windows_ping_ubuntu.png
* wireshark_icmp_capture.png
* wireshark_packet_details.png
* wireshark_icmp_live_capture.png
* ssh_windows_to_ubuntu.png
* sshd_service_running.png
```

### Infrastructure

* VirtualBox dashboard showing both VMs
* Ubuntu VM running
* Windows VM running

### Network Configuration

* Ubuntu `ip addr`
* Windows `ipconfig`
* Routing table verification
* NAT configuration

### Connectivity Testing

* Ubuntu pinging Windows
* Windows pinging Ubuntu

### Packet Analysis

* Wireshark ICMP captures
* Packet inspection

## SSH Remote Access Configuration

### Objective
Enable secure remote login between Windows and Ubuntu virtual machines using SSH.

---

### Steps Performed
- Installed and configured OpenSSH server on Ubuntu
- Verified that the SSH service was running (`systemctl status ssh`)
- Used Windows PowerShell SSH client to connect to Ubuntu
- Established connection using Ubuntu username and IP address
- Successfully authenticated using Ubuntu credentials

---

### Verification
- SSH login was successful from Windows to Ubuntu
- Full interactive Linux shell access was achieved
- Confirmed remote command-line access to Ubuntu VM

---

### Key Observations
- Incorrect SSH syntax initially caused connection failure
- Correct format (`username@ip-address`) resolved the issue
- Network connectivity between both virtual machines was confirmed
- SSH service must be active and listening on port 22 for connection success

---

### Skills Practiced
- Secure Shell (SSH) protocol usage
- Remote system administration
- Network troubleshooting and debugging
- Cross-platform connectivity (Windows and Linux)
- Client-server communication over TCP/IP

## Reverse SSH Configuration (Ubuntu → Windows)

### Objective

Enable remote SSH access from the Ubuntu virtual machine into the Windows virtual machine.

---

### Purpose

This exercise demonstrated cross-platform remote administration between Linux and Windows systems using the SSH protocol.

---

## Windows OpenSSH Server Installation

### Verification of OpenSSH Components

Checked whether OpenSSH Client and Server were installed using PowerShell:

```powershell
Get-WindowsCapability -Online | Where-Object Name -like "OpenSSH*"
```

### Result

* OpenSSH Client → Installed
* OpenSSH Server → Installed

---

## Starting the SSH Service

### Command Used

```powershell
Start-Service sshd
```

### Configure Automatic Startup

```powershell
Set-Service -Name sshd -StartupType Automatic
```

### Verification

```powershell
Get-Service sshd
```

### Result

The SSH service status changed to:

```text
Status : Running
```

---

## Firewall Configuration

### Problem Encountered

Initial SSH connection attempts from Ubuntu timed out even though the SSH service was running.

### Cause

Windows Defender Firewall was blocking inbound SSH traffic on TCP port 22.

### Solution

Created a firewall rule to allow SSH connections:

```powershell
netsh advfirewall firewall add rule name="OpenSSH Server" dir=in action=allow protocol=TCP localport=22
```

### Outcome

Ubuntu was able to successfully reach the Windows SSH service after the firewall rule was added.

---

## SSH Connection Test from Ubuntu

### Command Used

```bash
ssh Dee@192.168.56.10
```

### Initial Warning

Ubuntu displayed a host authenticity warning because the Windows SSH host key had never been seen before.

### Action Taken

Accepted the SSH fingerprint by typing:

```text
yes
```

### Result

The Windows host key was added to Ubuntu’s known hosts file.

---

## Authentication Troubleshooting

### Problem Encountered

SSH authentication initially failed with:

```text
Permission denied (publickey,password,keyboard-interactive)
```

### Cause

Incorrect Windows username/password combination was used during authentication.

### Resolution

Verified the correct Windows username and retried the SSH connection using valid Windows credentials.

---

### Skills Practiced

* SSH server installation
* Windows service management
* Firewall rule configuration
* Cross-platform remote access
* Linux-to-Windows connectivity
* Authentication troubleshooting
* Secure remote administration

---

### Evidence Captured

* OpenSSH Server installed
* SSH service running (`sshd`)
* Windows firewall rule configuration
* Successful Ubuntu → Windows SSH connection
* Authentication prompts and troubleshooting

---

### Key Learning Outcomes

* Windows can function as an SSH server using OpenSSH
* Firewall configuration is critical for remote access
* SSH host key verification improves connection security
* Linux systems can securely administer Windows systems remotely
* Authentication failures are commonly caused by incorrect usernames or passwords


### Development Environment

* GitHub repository
* VS Code documentation
* Project folder structure

---

## Skills Practiced

### Virtualization

* Virtual machine deployment
* Virtual hardware configuration
* Multi-OS environment management

### Networking

* NAT networking
* Internal networking
* DHCP troubleshooting
* Static IP addressing
* Routing analysis
* ICMP testing

### Linux Administration

* NetworkManager usage
* `nmcli`
* `ip addr`
* `ip route`
* Netplan troubleshooting

### Windows Administration

* Network configuration
* Firewall management
* ICMP rule configuration

### Packet Analysis

* Wireshark captures
* Traffic inspection
* ICMP packet analysis

### Documentation & Version Control

* Markdown documentation
* Git usage
* GitHub repository management

---

### Lessons Learned

* VirtualBox Internal Network does not provide DHCP automatically
* Interface names can change depending on adapter order
* NAT and Internal Network serve different purposes
* Windows Firewall can block ICMP traffic
* Network troubleshooting requires systematic verification
* Accurate documentation improves troubleshooting efficiency

---

# Final Lab Architecture

| System      | Adapter 1 (NAT) | Adapter 2 (Internal Network) |
|-------------|-----------------|-------------------------------|
| Ubuntu VM   | 10.0.2.15       | 192.168.56.20               |
| Windows VM  | 10.0.2.15       | 192.168.56.10               |

## Connectivity Verified

| Test | Status |
|------|--------|
| Ubuntu → Internet | Successful |
| Windows → Internet | Successful |
| Ubuntu ↔ Windows Ping | Successful |
| Windows → Ubuntu SSH | Successful |
| Ubuntu → Windows SSH | Successful |
| Wireshark ICMP Capture | Successful |

### Current Status

Virtual networking lab successfully completed.

Implemented:

* Ubuntu VM
* Windows 11 VM
* NAT internet connectivity
* Internal VM communication
* Static IP addressing
* ICMP testing
* Wireshark packet capture
* GitHub documentation
* SSH configuration
* Advanced Linux administration
* Network monitoring exercises
