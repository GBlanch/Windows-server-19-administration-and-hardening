# Encrypting File System and Recovery Agent

## 1. Local EFS Implementation

The purpose of this initial phase was to demonstrate how Windows Server 2019 encrypts data at the file-system level using the Encrypting File System (EFS). 

This lab section began under the local Administrator account, where a desktop folder named SecretData was created and configured with the option Encrypt contents to secure data. From that point onward, every file placed inside the folder became automatically encrypted through the Administrator’s EFS certificate. A sample text document titled admin.txt was then saved in this directory to validate encryption behavior.

Next, a secondary local user account named stud1 was created and signed in to attempt accessing the encrypted file. When stud1 navigated to the Administrator’s desktop and opened the SecretData directory, the folder name appeared in green, confirming that encryption was active. Attempting to open admin.txt resulted in an “Access Denied” message, showing that EFS restricts decryption strictly to the certificate owner.

<p align="center"> <img src="screenshots/efs-ra-1.png" alt="VMware Inventory View" width="600"><br> <b>Image 1 – Report Details for MS1 within the lab NAT segment</b> </p>

To reinforce the concept of user-specific encryption, stud1 then created a personal folder on the desktop and repeated the process, generating an encrypted file called stud1.txt. After signing out, the Administrator attempted to open this file but was unable to decrypt its contents, confirming that each user’s EFS certificate is unique and cannot decrypt another user’s data.

<p align="center"> <img src="screenshots/efs-ra-2.png" alt="VMware Inventory View" width="600"><br> <b>Image 2 – Report Details for MS1 within the lab NAT segment</b> </p>

## 2. Command-Line Encryption with Cipher

This part focused on using the Cipher tool to manage encryption from the command line instead of through the graphical interface. 

While logged in as Administrator, a new folder named _mywork_ was created on the desktop. Running `cipher mywork\*` showed the encryption status of the files inside. Items marked with E were encrypted, while those labeled U were still unencrypted. The same command was used on the _SecretData_ folder, confirming that admin.txt had the E attribute.

Next, a folder named _private_ was created with a subfolder personal and a file called _mydata.txt._ The command
`cipher /e /s:private`
was entered to encrypt both the folder and its contents. The `/e` option starts encryption, and `/s` includes all subfolders. After that, the personal folder turned green in File Explorer, showing encryption was active. Any new file saved inside was automatically protected.

<p align="center"> <img src="screenshots/efs-ra-3.png" alt="VMware Inventory View" width="600"><br> <b>Image 2 – Report Details for MS1 within the lab NAT segment</b> </p>