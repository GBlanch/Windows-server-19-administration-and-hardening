# Just Enough Administration (JEA)
## 1. Introduction

This lab focused on implementing JEA in a Windows Server 2016 environment to demonstrate how administrative privileges can be securely delegated. The exercise was performed within the existing Active Directory domain, using the servers DC1 and WS1.

The goal was to configure PowerShell-based role definitions that allow a specific group to perform only the DNS management tasks required, without granting full administrative access. Through the creation of role capability files, session configuration files, and custom PowerShell endpoints, the lab illustrated how JEA enforces the least privilege principle while maintaining centralized control and traceability of actions.

The configuration and testing process took place across both systems — with DC1 acting as the domain controller and configuration host, and WS1 used to validate the delegated access through remote PowerShell sessions.

## 2. Creating and Configuring Role Capability Files

This stage began on DC1, where the PowerShell module directory was prepared for a new JEA role named DNSOps. Using PowerShell ISE, the Administrator navigated to the system module path and created both the module manifest and the role capability file. These steps established the foundation for defining which commands members of the DNSOps group would be permitted to execute.

After creating the required directories and cd-ing into them, two key files were generated. The first, `DNSOps.psd1`, is the module manifest that defines the metadata and structure of the module. The second, `DNSOps.psrc`, is the role capability file, which specifies the commands and functions available to users under the DNSOps JEA role:

```
New-ModuleManifest .\DNSOps.psd1
```
```

New-PSRoleCapabilityFile -Path .\DNSOps.psrc
```


Once created, the capability file was opened and customized. Under the _#VisibleCmdlets_ section, the Administrator defined the DNS-related cmdlets that users would be allowed to run, such as restarting the DNS service. The _VisibleFunctions_ and _VisibleExternalCommands_ sections were also configured to include DNS management functions.

After editing, the file was saved to finalize the configuration of the _DNSOps.psrc_ capability file.

<p align="center"> <img src="screenshots/jea-1.png" alt="VMware Inventory View" width="600"><br> <b>Image 1 – Creating / Editing the DNSOps.psrc role capability file in PowerShell ISE</b> </p>

This configuration effectively restricted JEA session users to only the specified DNS-related cmdlets and external commands, preventing any unauthorized administrative actions beyond their assigned role.

## 3. Creating and Configuring Session Configuration Files

Once the role capability file was defined, the next part involved creating a session configuration file to control how the JEA environment would be applied. Working on DC1, the Administrator opened PowerShell ISE and generated a new session configuration file named `DNSOps.pssc` using the `New-PSSessionConfigurationFile -Path .\DNSOps.pssc -Full` command. This file acts as a blueprint that determines which cmdlets, parameters, and functions are allowed when connecting to the JEA endpoint.

In the PowerShell ISE editor, several modifications were made to restrict and secure the session. The line defining the session type was changed from `SessionType = 'Default'` to `SessionType = 'RestrictedRemoteServer'`, limiting command availability. The parameter `RunAsVirtualAccount` was enabled by uncommenting it and setting it to `$true`, ensuring that temporary virtual credentials would be used for elevated tasks. Finally, under the section for role definitions, the DNSOps group was mapped to the DNSOps capability file created earlier, establishing the relationship between the user group and its allowed commands.

After all changes were saved, the configuration effectively tied the DNSOps role capabilities to a restricted PowerShell session for future connections.

<p align="center"> <img src="screenshots/jea-2.png" alt="VMware Inventory View" width="600"><br> <b>Image 2 – Editing DNSOps.pssc session configuration</b> </p>

# 4. Creating and Connecting to a JEA Endpoint

After completing the configuration files, the next phase involved registering and testing the JEA endpoint. Still working on DC1, the Administrator created a new security group named DNSOps in Active Directory and added the delegated user account as a member. This group would later be associated with the JEA configuration, allowing only its members to connect and execute DNS management commands.

A PowerShell session was opened to register the configuration file as a new JEA endpoint. Using the command `Register-PSSessionConfiguration -Name DNSOps -Path .\DNSOps.pssc`, the Administrator linked the previously defined session configuration to the system. Once the registration completed successfully, the WinRM service was restarted with `Restart-Service WinRM` to apply the new endpoint configuration. The command `Get-PSSessionConfiguration` was then executed to confirm that the DNSOps endpoint was active and available.

# 5. Testing Remote JEA Connections

With the JEA endpoint registered and verified, the next step was to confirm that the role restrictions functioned as expected during remote PowerShell sessions. On WS1, the delegated user connected to DC1 using the command `Enter-PSSession -ComputerName DC1 -ConfigurationName DNSOps`. Once connected, the command `(Get-Command).count` was executed to verify that only a limited set of cmdlets was available, corresponding to those explicitly defined in the DNSOps role capability file.

The connection successfully permitted DNS-related commands, such as `Get-DNSServerResourceRecord`, `Add-DNSServerResourceRecord`, and `Clear-DNSServerCache`. Each of these operations ran without errors, confirming that the role capability configuration was functioning correctly and providing access solely to DNS management tasks.

Next, two common administrative commands — `Get-Service` and `Restart-Service` — were tested intentionally to verify restriction enforcement. Both commands returned an error message indicating that they were not recognized or allowed within the session. This demonstrated that the JEA endpoint successfully prevented users from running unauthorized cmdlets that were not part of the VisibleFunctions list defined in the DNSOps role capability file.


<p align="center"> <img src="screenshots/jea-.png" alt="VMware Inventory View" width="600"><br> <b>Image 2 – TBC</b> </p>

Through this verification, the lab confirmed that the JEA configuration enforced the least-privilege model effectively, allowing only the required DNS management capabilities while denying all other administrative operations.