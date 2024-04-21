# AD Administration: Guided Lab Part I

Let us consider the following case:

![Image description](image)

In this section, we will serve as domain administrators to Inlanefreight for a day. We have been tasked to help the IT department close some work orders, so we will be performing actions such as adding and removing users and groups, managing group policy, and more. Successful completion of the tasks can lead to us gaining a promotion to the Tier II IT team from the helpdesk.

## Connection Instructions

For this lab, you will have access to a domain-joined Windows server from which you can perform any actions needed to complete the lab. The environment will require you to RDP from Pwnbox or your own VM over VPN to the Windows server. Follow the steps below to utilize RDP and connect to the lab's Windows host.

Click below in the Questions section to spawn the target host and obtain an IP address. The image below shows where to spawn the target and acquire a VPN key for the lab if needed.

IP ==
Username == htb-student_adm
Password == Academy_student_DA!

We will use xfreerdp to connect with the target.

Open a terminal in pwnbox or from your lab vm over vpn and enter the following command:

```bash
xfreerdp /v: /u:htb-student_adm /p:Academy_student_DA!
```

Once connected, open an MMC console, PowerShell, or the ADDS tools to begin.

## Tasks

Attempt to complete the challenges on your own. If you get stuck, the Solutions dropdown below each task can help you. This reference on the [Active Directory PowerShell module](link) will be extremely helpful. As an introductory course on AD, we do not expect you to know everything about the topic and how to administer it. The Solutions below each task offer a step-by-step of how to complete the task. This section is provided to give you a taste of the daily tasks that AD administrators perform. Instead of providing the information to you in a static format, we have opted to provide it in a more hands-on manner.

### Task 1: Manage Users

Our first task of the day includes adding a few new-hire users into AD. We are just going to create them under the "inlanefreight.local" scope, drilling down into the "Corp > Employees > HQ-NYC > IT " folder structure for now. Once we create our other groups, we will move them into the new folders. You can utilize the Active Directory PowerShell module (`New-ADUser`), the Active Directory Users and Computers snap-in, or MMC to perform these actions.

Users to Add:

- Andromeda Cepheus
- Orion Starchaser
- Artemis Callisto

Each user should have the following attributes set, along with their name:

| Attribute                               | Value                                                                          |
| --------------------------------------- | ------------------------------------------------------------------------------ |
| full name                               | [User's Full Name]                                                             |
| email                                   | first-initial.lastname@inlanefreight.local (e.g., j.smith@inlanefreight.local) |
| display name                            | [User's Display Name]                                                          |
| User must change password at next logon | Checked                                                                        |

Once we have added our new hires, take a quick second and remove a few old user accounts found in an audit that are no longer required.

Users to Remove:

- Mike O'Hare
- Paul Valencia

Lastly, Adam Masters has submitted a trouble ticket over the phone saying his account is locked because he typed his password wrong too many times. The helpdesk has verified his identity and that his Cyber awareness training is up to date. The ticket requests that you unlock his user account and force him to change his password at the next login.

![Image description](image)

#### Solution: Task 1

Open PowerShell as an administrator. To ADD a user into Active Directory, we First need to load the module with the `"Import-Module -Name ActiveDirectory"` cmdlet. The AD module can be installed via the RSAT feature pack, but for now, it's already installed on the host used in this lab.

PowerShell Terminal Output for Adding a User

```powershell
PS C:\htb> New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"}
```

After you hit enter, a prompt will appear, enter a secure password for the user.

Adding a User from the MMC Snap-in

Before adding a user from the GUI we need to open the Active Directory Users and Computers (ADUC) MMC tool. As a standard user, we may have access to view the ADUC objects, but we will not be able to modify or add. We need to log in as our administrator account (credentials above) to complete these actions. Once logged in, open the ADUC snap-in by performing the following actions:

- From the Server Manager window, select Tools > then ADUC
- Expand the scope "inlanefreight.local" and drill down to "Corp > Employees > HQ-NYC > IT ". This is where we will be creating our new users, OU's, and Groups.

Adding an AD User via the GUI

To add an AD user via the GUI we first need to open Active Directory Users and Computers via the Start Menu folder Administrative Tools.

1. Right-click on "IT", Select "New" > "User".

![Add A User](image)

We will add the new user Andromeda Cepheus to our domain. We can do so by:

- Right-click on "IT" > Select "New" > "User". A popup window will appear with a field for you to fill in.
- Add the user's First and Last name, set the "User Logon Name:" as acepheus, and then hit Next.
- Now supply the new user with a password of NewP@ssw0rd123!, confirm the password again, and check the box for " User must change password at next login", then hit next. Select "Finish" in the last window if all attributes look correct.

To REMOVE a user account from Active Directory, we can:

PowerShell to Remove a User

```powershell
PS C:\htb> Remove-ADUser -Identity pvalencia
```

The `Remove-ADUser` cmdlet above targets the user by its user logon name. Ensure you are targeting the right user before executing it. If we are unsure of the value needed, we can use the `Get-ADUser` command to validate first.

Remove a User from the MMC Snap-in

Now we will remove a user Paul Valencia from our domain. We can do so by:

The most straightforward method from the ADUC snap-in will be to use the find functionality. Inlanefreight has many users across several OU's. To use find:

- Right-click on Employees and select "find".
- Type in the username you wish to search for, in this case, "Paul Valencia" and hit "Find Now." If a user has that name, the search results will appear lower in the find window.
- Now, right-click on the user and select delete. A popup window will appear to confirm the deletion of the user. Hit yes.
- To validate the user is deleted, you can use the Find feature again to search for the user.

Deleting a User via the GUI

To delete a user via the GUI, we will use the ADUC snap-in just like when we added a user to the domain above.

1. Right-click on the "Employees OU" and select "find".

![Image description](image)

Now we need to help Adam Masters out and unlock his account again.

To UNLOCK a user account we can:

PowerShell To Unlock a User

```powershell
PS C:\htb> Unlock-ADAccount -Identity amasters
```

We also need to set a new password for the user and force them to change the password at the next logon. We will do this with the `SetADAccountPassword` and `Set-ADUser` cmdlets.

Reset User Password (`Set-ADAccountPassword`)

```powershell
PS C:\htb> Set-ADAccountPassword -Identity 'amasters' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)
```

Force Password Change (`Set-ADUser`)

```powershell
PS C:\htb> Set-ADUser -Identity amasters -ChangePasswordAtLogon $true
```

Unlock from Snap-in

Unlocking this user account will take several steps. The first is to unlock the account, then we set it so that the user must change his password at the next login, and then we
