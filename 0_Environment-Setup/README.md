# Environment Setup

## 1. Overview
<br>
This environment was created to serve as the foundation for all upcoming Windows Server configuration and hardening work. It runs locally on VMware Workstation using both a host-only and a NAT network to simulate a small enterprise setup. I've aimed to follow the most solid practices for server network architecture and their security.

<br>
The host-only network operates on 192.168.45.0/24, while the NAT network uses 192.168.200.0/24. The base system, named Windows Server (WS1), is a clean Windows Server 2019 installation that will later be cloned into other machines such as Domain Controller 1 (DC1) and Client/Member Server (MS1).
<br>

This document is organized into the following sections:

- System Requirements: hardware and software prerequisites.

- VMware Network Configuration: details of the virtual network setup.

- Windows Server Installation: installation process and base system notes.

- Snapshot and Backup Strategy: methods to preserve configuration states.

- Systems verification: confirming the environment is ready.

<br>

I tried to include in each section a screenshot of the subject to showcase better.

## 2. System Requirements
<br>

| Component | Minimum | Recommended |
|------------|----------|--------------|
| Host OS | Windows 10/11 | Windows 11 Pro |
| Virtualization | VMware Workstation | VMware Workstation Pro 17+ |
| RAM | 8 GB | 16 GB+ |
| Storage | ~ 60 GB | SSD ~ 100 GB+ |
| Network | VMnet1 (Host-Only), VMnet8 (NAT) | Static IP ranges configured |

## 3. VMware Network Configuration

The virtual environment is designed with two routing layers to ensure strong isolation and controlled access. 
The first router is the main household LAN router, which provides internet connectivity to the host system. The second is an intermediate NAT router with a built-in firewall, responsible for creating a dedicated private network for all lab virtual machines.

A bastion host is positioned between these two networks, acting as the secure bridge between the household LAN and the isolated lab segment. It provides controlled administrative SSH access without exposing internal systems directly.


Within this private network, VMnet1 operates as Host-Only on 192.168.45.0/24, while VMnet8 runs in NAT mode on 192.168.200.0/24. In order to stable balance between isolation and external / temporary connectivity, DHCP is disabled on VMnet1 and enabled on VMnet8 initially.

![VMnet1 Configuration](../screenshots/vmnet1.png)
<br>

![VMnet1 Configuration](../screenshots/vmnet1.png)

This layered aims to ensure that the lab remains fully segregated from the physical LAN, while the bastion host and 2nd NAT firewall router enables safe management and selective outbound connectivity, when required.

## 4. Windows Server Installation
<br>
To make summary of ISO source, product key, and base install steps.  

*(Reference detailed guide in `Windows_Server_Installation.md`.)*

## 5. Snapshot and Backup Strategy
<br>
To define when to snapshot and where to store VM copies for recovery.

## 6. Systems verification
<br>
to confirm the environment is operational 

## 8. Notes & References
Additional remarks, version notes, and official documentation links.
