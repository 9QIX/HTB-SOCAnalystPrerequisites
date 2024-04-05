# Finding & Filtering Content

Being able to search for, find, and filter content for what we are looking for is an absolute requirement for any user who utilizes the CLI ( regardless of what shell or OS ). Nevertheless, how do we do this in PowerShell? To answer this question, this section will dive into specifics of how PowerShell utilizes Objects, how we can filter based on Properties and content, and describe components like the PowerShell Pipeline further.

## Explanation of PowerShell Output (Objects Explained)

With PowerShell, not everything is generic text strings like in Bash or cmd. In PowerShell, everything is an Object. However, what is an object? Let us examine this concept further:

**What is an Object?** An object is an individual instance of a class within PowerShell. Let us use the example of a computer as our object. The total of everything (parts, time, design, software, etc.) makes a computer a computer.

**What is a Class?** A class is the schema or 'unique representation of a thing (object) and how the sum of its properties define it. The blueprint used to lay out how that computer should be assembled and what everything within it can be considered a Class.

**What are Properties?** Properties are simply the data associated with an object in PowerShell. For our example of a computer, the individual parts that we assemble to make the computer are its properties. Each part serves a purpose and has a unique use within the object.

**What are Methods?** Simply put, methods are all the functions our object has. Our computer allows us to process data, surf the internet, learn new skills, etc. All of these are the methods for our object.

Now, we defined these terms so that we understand all the different properties we will be looking at later and what methods of interaction we have with objects. By understanding how PowerShell interprets objects and utilizes Classes, we can define our own object types. Moving on, we will look at how we can filter and find objects through the PowerShell CLI.

## Finding and Filtering Objects

Let us look at this through a user object context. A user can do things like access files, run applications, and input/output data. But what is a user? What is it made up of?

### Get an Object (User) and its Properties/Methods

```powershell
PS C:\htb> Get-LocalUser administrator | get-member

   TypeName: Microsoft.PowerShell.Commands.LocalUser

Name                   MemberType Definition
----                   ---------- ----------
Clone                  Method     Microsoft.PowerShell.Commands.LocalUser Clone()
Equals                 Method     bool Equals(System.Object obj)
GetHashCode            Method     int GetHashCode()
GetType                Method     type GetType()
ToString               Method     string ToString()
AccountExpires         Property   System.Nullable[datetime] AccountExpires {get;set;}
Description            Property   string Description {get;set;}
Enabled                Property   bool Enabled {get;set;}
FullName               Property   string FullName {get;set;}
LastLogon              Property   System.Nullable[datetime] LastLogon {get;set;}
Name                   Property   string Name {get;set;}
ObjectClass            Property   string ObjectClass {get;set;}
PasswordChangeableDate Property   System.Nullable[datetime] PasswordChangeableDate {get;set;}
PasswordExpires        Property   System.Nullable[datetime] PasswordExpires {get;set;}
PasswordLastSet        Property   System.Nullable[datetime] PasswordLastSet {get;set;}
PasswordRequired       Property   bool PasswordRequired {get;set;}
PrincipalSource        Property   System.Nullable[Microsoft.PowerShell.Commands.PrincipalSource] PrincipalSource {ge...
SID                    Property   System.Security.Principal.SecurityIdentifier SID {get;set;}
UserMayChangePassword  Property   bool UserMayChangePassword {get;set;}
```

Now that we can see all of a user's properties let us look at what those properties look like when output by PowerShell. The `Select-Object` cmdlet will help us achieve this. In this manner, we now understand what makes up a user object.

### Property Output (All)

```powershell
PS C:\htb> Get-LocalUser administrator | Select-Object -Property *


AccountExpires         :
Description            : Built-in account for administering the computer/domain
Enabled                : False
FullName               :
PasswordChangeableDate :
PasswordExpires        :
UserMayChangePassword  : True
PasswordRequired       : True
PasswordLastSet        :
LastLogon              : 1/20/2021 5:39:14 PM
Name                   : Administrator
SID                    : S-1-5-21-3916821513-3027319641-390562114-500
PrincipalSource        : Local
ObjectClass            : User
```

A user is a small object realistically, but it can be a lot to look at the output in this manner, especially from items like large lists or tables. So what if we wanted to filter this content down or show it to us in a more precise manner? We could filter out the properties of an object we do not want to see by selecting the few we do. Let's look at our users and see which have set a password recently.

### Filtering on Properties

```powershell
PS C:\htb> Get-LocalUser * | Select-Object -Property Name,PasswordLastSet

Name               PasswordLastSet
----               ---------------
Administrator
DefaultAccount
Guest
MTanaka              1/27/2021 2:39:55 PM
WDAGUtilityAccount 1/18/2021 7:40:22 AM
```

We can also sort and group our objects on these properties.

### Sorting and Grouping

```powershell
PS C:\htb> Get-LocalUser * | Sort-Object -Property Name | Group-Object -property Enabled

Count Name                      Group
----- ----                      -----
    4 False                     {Administrator, DefaultAccount, Guest, WDAGUtilityAccount}
    1 True                      {MTanaka}
```

We utilized the `Sort-Object` and `Group-Object` cmdlets to find all users, sort them by name, and then group them together based on their Enabled property. From the output, we can see that several users are disabled and not in use for interactive logon. This is just a quick example of what can be done with PowerShell objects and the sheer amount of information stored within each object. As we delve deeper into PowerShell and dig around within the Windows OS, we will notice that the classes behind many objects are extensive and often shared. Keep these things in mind as we work with them more and more.

## Why Do We Need to Filter our Results?

We are switching it up and using an example of `Get-Service` for this demonstration. Looking at basic users and information does not produce much in the way of results, but other objects contain an extraordinary amount of data. Below is an example of just a fragment from the output of `Get-Service`:

### Too Much Output

```powershell
PS C:\htb> Get-Service | Select-Object -Property *

Name                : AarSvc_1ca8ea
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Agent Activation Runtime_1ca8ea
DependentServices   : {}
MachineName         : .
ServiceName         : AarSvc_1ca8ea
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Stopped
ServiceType         : 224
StartType           : Manual
Site                :
Container           :

Name                : AdobeARMservice
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : Adobe Acrobat Update Service
DependentServices   : {}
MachineName         : .
ServiceName         : AdobeARMservice
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Running
ServiceType         : Win32OwnProcess
StartType           : Automatic
Site                :
Container           :

Name                : agent_ovpnconnect
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : OpenVPN Agent agent_ovpnconnect
DependentServices   : {}
MachineName         : .
ServiceName         : agent_ovpnconnect
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Running
ServiceType         : Win32OwnProcess
StartType           : Automatic
Site                :
Container           :
```

This is way too much data to sift through, right? Let us break it down further and format this data as a list. We can use the command string `Get-Service | Select-Object -Property DisplayName,Name,Status | Sort-Object DisplayName | Format-List` to change our output like so:

```powershell
PS C:\htb> Get-Service | Select-Object -Property DisplayName,Name,Status | Sort-Object DisplayName | Format-List

DisplayName : ActiveX Installer (AxInstSV)
Name        : AxInstSV
Status      : Stopped

DisplayName : Adobe Acrobat Update Service
Name        : AdobeARMservice
Status      : Running

DisplayName : Adobe Genuine Monitor Service
Name        : AGMService
Status      : Running
```

This is still a ton of output, but it is a bit more readable. Here is where we start asking ourselves questions like do we need all of this output? Do we care about all of these objects or just a specific subset of them? What if we wanted to determine if a specific service was running, but we needed to figure out the specific Name? The `Where-Object` can evaluate objects passed to it and their specific property values to look for the information we require. Consider this scenario:

**Scenario:** We have just landed an initial shell on a host via an unsecured protocol exposing the host to the world. Before we get any further in, we need to assess the host and determine if any defensive services or applications are running. First, we look for any instance of `Windows Defender` services running.

Using `Where-Object` (where as an alias) and the parameter matching with `-like` will allow us to determine if we are safe to continue by looking for anything with "Defender" in the property. In this instance, we check the `DisplayName` property of all objects retrieved by `Get-Service`.

### Hunting for Windows Defender

```powershell
PS C:\htb>  Get-Service | where DisplayName -like '*Defender*'

Status   Name               DisplayName
------   ----               -----------
Running  mpssvc             Windows Defender Firewall
Stopped  Sense              Windows Defender Advanced Threat Pr...
Running  WdNisSvc           Microsoft Defender Antivirus Networ...
Running  WinDefend          Microsoft Defender Antivirus Service
```

As we can see, our results returned several services running, including Defender Firewall, Advanced Threat Protection, and more. This is both good news and bad news for us. We cannot just dive in and start doing things because we are likely to be spotted by the defensive services, but it is good that we spotted them and can now regroup and make a plan for defensive evasion actions to be taken. Although a quick example scenario, this is something as pentesters that we will often run into, and we should be able to spot and identify when defensive measures are in place. This example brings up an interesting way to modify our searches, however. Evaluation values can be beneficial to our cause. Let us check them out more.

## The Evaluation of Values

`Where` and many other cmdlets can evaluate objects and data based on the values those objects and their properties contain. The output above is an excellent example of this utilizing the `-like` Comparison operator. It will look for anything that matches the values expressed and can include wildcards such as `*`. Below is a quick list (not all-encompassing) of other useful expressions we can utilize:

### Comparison Operators

| Expression | Description                                                                                                                                             |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Like       | Like utilizes wildcard expressions to perform matching. For example, `'*Defender*'` would match anything with the word Defender somewhere in the value. |
| Contains   | Contains will get the object if any item in the property value matches exactly as specified.                                                            |
| Equal to   | Specifies an exact match (case sensitive) to the property value supplied.                                                                               |
| Match      | Is a regular expression match to the value supplied.                                                                                                    |
| Not        | specifies a match if the property is blank or does not exist. It will also match $False.                                                                |

Of course, there are many other comparison operators we can use like, greater than, less than, and negatives like `NotEqual`, but in this kind of searching they may not be as widely used. Now with a `-GTE` understanding of how these operators can help us more than before (see what I did there), let us get back to digging into Defender services. Now we will look for service objects with a `DisplayName` again, like `< something>Defender< something>`.

### Defender Specifics

```powershell
PS C:\htb> Get-Service | where DisplayName -like '*Defender*' | Select-Object -Property *

Name                : mpssvc
RequiredServices    : {mpsdrv, bfe}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Windows Defender Firewall
DependentServices   :
MachineName         : .
ServiceName         : mpssvc
ServicesDependedOn  : {mpsdrv, bfe}
ServiceHandle       :
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Automatic
Site                :
Container           :

Name                : Sense
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Windows Defender Advanced Threat Protection Service
```

Our results above now filter out every service associated with Windows Defender and displays the complete properties list of each match. Now we can look at the services, determine if they are running, and even if we can, at our current permission level, affect the status of those services (turn them off, disable them, etc.). During many of the commands we have issued in the last few sections, we have used the `|` symbol to concatenate multiple commands we would usually issue separately. Below we will discuss what this is and how it works for us.

## What is the PowerShell Pipeline? ( | )

In its simplest form, the Pipeline in PowerShell provides the end user a way to chain commands together. This chain is called a Pipeline and is also referred to as a pipe or piping commands together. With PowerShell handling objects the way it does, we can issue a command and then pipe (`|`) the resultant object output to another command for action. The Pipeline will interpret and execute the commands one at a time from left to right. We have done this in a few examples in the previous sections, so we are diving deeper into it here. As an example using the Pipeline to string commands together can look like this:

### Piping Commands

```powershell
PS C:\htb> Command-1 | Command-2 | Command-3

Output from the result of 1+2+3
```

OR

```powershell
PS C:\htb>
Command-1 |
  Command-2 |
    Command-3

Output result from Pipeline
```

OR

```powershell
PS C:\htb> Get-Process | Where-Object CPU | Where-Object Path
    | Get-Item |

Output result from Pipeline
```

Each way is a perfectly acceptable way to concatenate the commands together. PowerShell can interpret what you want based on the position of the `(|)` in the string. Let us see an example of using the pipeline to provide us with actionable data. Below we will issue the `Get-Process` cmdlet, sort the resultant data, and then measure how many unique processes we have running on our host.

### Using the Pipeline to Count Unique Instances

```powershell
PS C:\htb> Get-Process | Sort-Object | Get-Unique | Measure-Object

Count             : 113
```

As a result, the pipeline output the total count (113) of unique processes running at that time. Suppose we break the pipeline down at any particular point. In that case, we may see the process output sorted, filtered for unique instances (no duplicate names), or just a number output from the `Measure-Object` cmdlet. The task we performed was relatively simple. However, what if we could harness this for something more complex, like sorting new log entries, filtering for specific event log codes, or processing large amounts of data (a database and all its entries, for example) looking for specific strings? This is where Pipeline can increase our productivity and streamline the output we receive, making it a vital tool for any sysadmin or pentester.

## Pipeline Chain Operators ( && and || )

_Currently, Windows PowerShell 5.1 and older do not support Pipeline Chain Operators used in this fashion. If you see errors, you must install PowerShell 7 alongside Windows PowerShell. They are not the same thing._

You can find a great example of [installing PowerShell 7 here](https://docs.microsoft.com/en
