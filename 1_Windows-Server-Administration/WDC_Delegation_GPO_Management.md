# Windows Domain Controller – Delegation and GPO Management
## 1. Overview

This markdown file presents the configuration and verification of Active Directory delegation and Group Policy Object management in a Windows Server domain. The objective was to assign specific administrative responsibilities through security groups, apply AppLocker and EFS Recovery Agent policies, and verify their effects from both the domain controller (DC1) and the member server (MS1).



## 2. Active Directory and Organizational Unit Structure

A logical domain structure was implemented to organize administrative boundaries. The domain included organizational units named Staff, Marketing, Sales, IT, and HelpDesk. These units defined where group policies and delegated permissions would apply, ensuring that administrative rights were scoped only to relevant containers. The structure aimed to establish a clear hierarchy suitable for role-based access control and subsequent delegation of permissions.

<p align="center"> <img src="screenshots/m.png" alt="VMware Inventory View" width="600"><br> <b>Image 2 – Password report details for DC1</b> </p>
