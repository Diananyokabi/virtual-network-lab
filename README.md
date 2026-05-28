# Virtual Network Lab (Enterprise Simulation Project)

This project simulates a small enterprise network environment using VirtualBox with Ubuntu and Windows VMs. It demonstrates practical skills in networking, system administration, and troubleshooting.

---

## 🎯 Project Overview

This project demonstrates practical IT skills by building a virtual networking environment using:

- Ubuntu Linux
- VirtualBox
- Wireshark
- macOS host system

The goal is to simulate real IT support and networking scenarios for learning and portfolio development.

---

## 🏗️ Lab Architecture

This virtual environment simulates a small internal network with internet access.

### Network Design

- NAT Network (Internet access)
  - Ubuntu VM
  - Windows VM

- Internal Network (labnet)
  - Ubuntu ↔ Windows communication
  - Static IP addressing

### IP Addressing Scheme

| VM      | NAT (Internet) | Internal Network |
|---------|----------------|------------------|
| Ubuntu  | 10.0.2.x       | 192.168.56.20    |
| Windows | 10.0.2.x       | 192.168.56.10    |

## 🔗 Network Flow

- NAT network provides internet access for both VMs
- Internal network enables isolated VM-to-VM communication
- SSH allows secure remote administration between systems
- ICMP used for connectivity testing

## 🧠 Key Learning Objectives

- Virtual machine setup and management
- Linux operating system fundamentals
- Network troubleshooting basics
- Packet capture and analysis using Wireshark
- Documentation and technical reporting
- Version control using Git and GitHub

---

## 🛠️ Tools & Technologies

- VirtualBox
- Ubuntu Desktop
- Wireshark
- Git & GitHub
- VS Code
- macOS

---

## 🔧 Key Technical Implementations

- Configured dual-network VirtualBox adapters (NAT + Internal Network)
- Implemented static IP addressing for VM-to-VM communication
- Resolved DHCP and routing issues in Linux (NetworkManager / nmcli)
- Configured Windows Firewall to allow ICMP traffic
- Enabled SSH server on Ubuntu and tested remote access from Windows
- Performed packet capture and ICMP analysis using Wireshark

## Results

- Successfully configured VM-to-VM networking
- Implemented static IP addressing
- Enabled SSH remote access between Linux and Windows
- Captured and analyzed network traffic using Wireshark
- Resolved DHCP, firewall, and routing issues

## 📁 Project Structure

```
Virtual-Network-Lab/
├── notes/         # Technical documentation and learning notes
├── screenshots/   # Evidence of setup and configurations
└── configs/       # Network/system configuration files
```

---

## 📸 Evidence & Documentation

This repository includes step-by-step documentation of:

- VirtualBox installation
- Ubuntu VM creation
- Git and GitHub setup
- Troubleshooting installation issues
- System configuration steps

All evidence is stored in the `screenshots/` folder.

---

## ⚠️ Issues Encountered & Resolutions

During setup, several real-world issues were solved:

- Missing macOS developer tools (resolved using Xcode Command Line Tools)
- GitHub authentication via browser login (due to password deprecation)
- Git merge conflicts between local and remote repositories

These reflect real IT troubleshooting scenarios.

---

## 📈 Skills Demonstrated

- Linux Administration (Ubuntu)
- Windows Administration
- TCP/IP Networking
- SSH Remote Access
- NAT & Internal Networking
- DHCP & Static IP Configuration
- Firewall Configuration
- Packet Analysis (Wireshark)
- Git & GitHub Version Control
- Network Troubleshooting Methodology

---

## 🚀 Next Steps

- Expand lab with additional servers (DNS / Web server)
- Simulate enterprise network segmentation
- Introduce firewall rule testing scenarios
- Explore basic penetration testing in isolated environment

---

## 🧭 Project Outcome

This project successfully demonstrates the ability to design, configure, and troubleshoot a small enterprise-style network environment using virtualization technologies.

It reflects real-world IT support and networking tasks including system setup, connectivity troubleshooting, and secure remote administration.

## 👤 Author

Built as a personal IT networking and support lab project for hands-on learning and portfolio development.
