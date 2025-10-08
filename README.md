

# Environment Setup

## 1. Overview
Brief description of the local virtualization environment used for Windows Server deployment and security hardening.

## 2. Objectives
- Prepare the host machine for virtualization  
- Configure VMware networking (Host-Only and NAT)  
- Install Windows Server 2019 base image  
- Create initial snapshots and backup strategy  

## 3. System Requirements
| Component | Minimum | Recommended |
|------------|----------|--------------|
| Host OS | Windows 10/11 | Windows 11 Pro |
| Virtualization | VMware Workstation | VMware Workstation Pro 17+ |
| RAM | 8 GB | 16 GB+ |
| Storage | ~ 60 GB | SSD ~ 100 GB+ |
| Network | VMnet1 (Host-Only), VMnet8 (NAT) | Static IP ranges configured |

## 4. VMware Network Configuration
to outline of subnet ranges, adapter modes, and DHCP settings.  
*(links to screenshots and VMnet configuration details.)*

## 5. Windows Server Installation
To make summary of ISO source, product key, and base install steps.  
*(Reference detailed guide in `Windows_Server_Installation.md`.)*

## 6. Snapshot and Backup Strategy
To define when to snapshot and where to store VM copies for recovery.

## 7. Verification
Checklist for confirming the environment is operational:
- [ ] VMware network connectivity  
- [ ] Windows Server boots successfully  
- [ ] Static IP addresses applied  
- [ ] Initial snapshot created  

## 8. Notes & References
Additional remarks, version notes, and official documentation links.
