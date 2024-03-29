# Skills Assessment - Windows Fundamentals

Inlanefreight recently had an incident where a disgruntled employee in marketing accessed an internally hosted HR share and deleted several confidential files & folders. Thankfully, the IT team had good backups and restored all of the deleted data. There are now concerns that this disgruntled employee was able to access the HR share in the first place. After performing a security assessment, you have found that the IT team may not fully understand how permissions work in Windows. You are conducting training and a demonstration to show the department good security practices with file sharing in a Windows environment as well as viewing services from the command line to check for any potential tampering.

### Step 1: Creating a shared folder called Company Data

To create a shared folder named "Company Data," follow these steps:

1. Right-click on the folder you want to share and select "Properties."
2. In the Properties window, navigate to the "Sharing" tab.
3. Click on the "Advanced Sharing" button.
4. Check the box that says "Share this folder."
5. Optionally, you can adjust the share name if you want it to differ from the folder name.
6. Click "Permissions" to set the permissions for the shared folder.
7. Add the appropriate users or groups and set their permissions accordingly.
8. Click "OK" to apply the changes and close the Sharing tab.

### Step 2: Creating a subfolder called HR inside of the Company Data folder

To create a subfolder named "HR" inside the "Company Data" folder, follow these steps:

1. Navigate to the location where you want to create the subfolder (e.g., "C:\Company Data").
2. Right-click in the folder and select "New" > "Folder."
3. Name the new folder "HR" and press Enter to create it.

### Step 3: Creating a user called Jim

To create a user named "Jim" without the requirement to change the password at logon, follow these steps:

1. Open the "Computer Management" console. You can do this by right-clicking on "This PC" or "My Computer," selecting "Manage."
2. In the Computer Management window, expand "System Tools" > "Local Users and Groups" > "Users."
3. Right-click in the right pane and select "New User."
4. Enter the user details, including the username (Jim), and uncheck the option "User must change password at next logon."
5. Click "Create" to create the user account.

### Step 4: Creating a security group called HR

To create a security group named "HR," follow these steps:

1. Open the "Computer Management" console if it's not already open.
2. Navigate to "Local Users and Groups" > "Groups."
3. Right-click in the right pane and select "New Group."
4. Enter the group name as "HR" and click "Create" to create the group.

### Step 5: Adding Jim to the HR security group

To add Jim to the HR security group, follow these steps:

1. Open the "Computer Management" console if it's not already open.
2. Navigate to "Local Users and Groups" > "Groups."
3. Double-click on the "HR" group to open its properties.
4. Click "Add" to add a new member to the group.
5. Enter "Jim" in the object names box and click "Check Names" to verify.
6. Click "OK" to add Jim to the HR group.

### Step 6: Adding the HR security group to the shared Company Data folder and NTFS permissions list

To add the HR security group to the shared "Company Data" folder and set NTFS permissions, follow these steps:

1. Right-click on the "Company Data" folder and select "Properties."
2. Navigate to the "Sharing" tab and click "Advanced Sharing."
3. Check the box for "Share this folder" and click "Permissions."
4. Remove the default group that is present.
5. Add the "HR" security group and set share permissions to "Allow Change" and "Read."
6. Click "OK" to close the Share Permissions window.
7. Back in the "Company Data Properties" window, click on the "Security" tab.
8. Click on the "Edit" button to change NTFS permissions.
9. Disable inheritance before issuing specific NTFS permissions.
10. Add the "HR" security group and set NTFS permissions to "Modify," "Read & Execute," "List folder contents," "Read," and "Write."
11. Click "OK" to apply the changes and close the Properties window.

### Step 7: Adding the HR security group to the NTFS permissions list of the HR subfolder

To add the HR security group to the NTFS permissions list of the "HR" subfolder, follow these steps:

1. Right-click on the "HR" subfolder and select "Properties."
2. Navigate to the "Security" tab and click "Edit" to change NTFS permissions.
3. Disable inheritance before issuing specific NTFS permissions.
4. Remove the default group that is present.
5. Add the "HR" security group and set NTFS permissions to "Modify," "Read & Execute," "List folder contents," "Read," and "Write."
6. Click "OK" to apply the changes and close the Properties window.

### Step 8: Using PowerShell to list details about a service

To list details about a service using PowerShell, follow these steps:

1. Open PowerShell as an administrator.
2. Use the `Get-Service` cmdlet followed by the service name to list details about the service.
   Example: `Get-Service -Name spooler`

This will display detailed information about the specified service, including its status, display name, service name, and service type.

That concludes the demonstration. If you have any questions or need further clarification, feel free to ask!

## Questions

- What is the name of the group that is present in the Company Data Share Permissions ACL by default?
- What is the name of the tab that allows you to configure NTFS permissions?
- What is the name of the service associated with Windows Update?

  ```powershell
  Get-Service | Where DisplayName -like “windows update”
  ```

- List the SID associated with the user account Jim you created.

  ```powershell
  Get-LocalUser -Name jim | Select-Object -Property *
  ```

- List the SID associated with the HR security group you created.

  ```powershell
  Get-LocalGroup -Name hr | Select-Object -Property *
  ```
