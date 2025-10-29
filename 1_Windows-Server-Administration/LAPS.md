# Local Administrator Password Solution (LAPS)

## 1. Configuring User Rights Assignment

This part began by accessing the Default Domain Controllers Policy through the Group Policy Management Console on DC1. The focus was to verify and adjust several user rights to ensure the correct administrative groups had the necessary privileges.

The Backup, Restore, and Shutdown permissions were reviewed and configured to include the Administrators, Backup Operators, and Server Operators groups. In addition, the Key Admins group was added where required to align with the security design of the domain.

Each policy was opened individually from the User Rights Assignment list, modified if necessary, and then applied. 

<p align="center"> <img src="screenshots/laps-1.png" alt="VMware Inventory View" width="600"><br> <b>Image 1 – VMware Inventory View</b> </p>


# 2 Creating and Linking GPOs

This part focused on building a structure to manage server-specific security policies. A new Organizational Unit named Servers was created in Active Directory to keep all managed machines under one container. After that, a new Group Policy Object called Remote Credential Guard was linked to it.

Inside the Group Policy Management Editor, the setting Restrict delegation of credentials to remote servers was enabled under the Credentials Delegation section. Because some versions of Windows Server do not show the “Prefer Remote Credential Guard” option, the “Require Remote Credential Guard” value was applied instead to achieve the same protection level, ensuring the policy strictly enforces credential protection and prevents fallback to less secure methods.

Once configured, the GPO was closed and saved, ensuring that any computer account later moved into the Servers OU would automatically inherit these protections.


<p align="center"> <img src="screenshots/laps-1.png" alt="VMware Inventory View" width="600"><br> <b>Image 1 – VMware Inventory View</b> </p>