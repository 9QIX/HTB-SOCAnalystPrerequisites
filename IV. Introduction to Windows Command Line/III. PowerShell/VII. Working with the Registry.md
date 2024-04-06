# Working with the Registry

## What Is The Windows Registry?

At its core, the Registry can be considered a hierarchal tree that contains two essential elements: keys and values. This tree stores all the required information for the operating system and the software installed to run under subtrees (think of them as branches of a tree). This information can be anything from settings to installation directories to specific options and values that determine how everything functions. As Pentesters, the Registry is a great spot to find helpful information, plant persistence, and more. MITRE provides many great examples of what a threat actor can do with access (locally or remotely) to a host's registry hive.

## What are Keys

Keys, in essence, are containers that represent a specific component of the PC. Keys can contain other keys and values as data. These entries can take many forms, and naming contexts only require that a Key be named using alphanumeric (printable) characters and is not case-sensitive. As a visual example of Keys, if we look at the image below, each folder within the Green rectangle is a Key and contains sub-keys.

![Keys (Green)](https://i.imgur.com/9V37PzD.png)

## Registry Key Files

A host systems Registry root keys are stored in several different files and can be accessed from `C:\Windows\System32\Config\`. Along with these Key files, registry hives are held throughout the host in various other places.

```powershell
PS C:\htb> Get-ChildItem C:\Windows\System32\config\

    Directory: C:\Windows\System32\config

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           12/7/2019  4:14 AM                Journal
d----           12/7/2019  4:14 AM                RegBack
d----           4/28/2021 11:43 AM                systemprofile
d----           9/18/2021 12:22 AM                TxR
-a---          10/12/2022 10:06 AM         786432 BBI
-a---           1/20/2021  5:13 PM          28672 BCD-Template
-a---          10/18/2022 11:14 AM       38273024 COMPONENTS
-a---          10/12/2022 10:06 AM        1048576 DEFAULT
-a---          10/15/2022  9:33 PM       13463552 DRIVERS
-a---           1/27/2021  2:54 PM          32768 ELAM
-a---          10/12/2022 10:06 AM         131072 SAM
-a---          10/12/2022 10:06 AM          65536 SECURITY
-a---          10/12/2022 10:06 AM      168034304 SOFTWARE
-a---          10/12/2022 10:06 AM       29884416 SYSTEM
-a---          10/12/2022 10:06 AM           1623 VSMIDK
```

For a detailed list of all Registry Hives and their supporting files within the OS, we can look [HERE](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-hives).

## What Are Values

Values represent data in the form of objects that pertain to that specific Key. These values consist of a name, a type specification, and the required data to identify what it's for. The image below visually represents Values as the data between the Orange lines. Those values are nested within the Installer key, which is, in turn, inside another key.

![Values](https://i.imgur.com/ER2O4Mu.png)

We can reference the complete list of Registry Key Values [HERE](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-value-types). In all, there are 11 different value types that can be configured.

## Registry Hives

Each Windows host has a set of predefined Registry keys that maintain the host and settings required for use. Below is a breakdown of each hive and what can be found referenced within.

| Name                | Abbreviation | Description                                                                                                                                                                                              |
| ------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HKEY_LOCAL_MACHINE  | HKLM         | This subtree contains information about the computer's physical state, such as hardware and operating system data, bus types, memory, device drivers, and more.                                          |
| HKEY_CURRENT_CONFIG | HKCC         | This section contains records for the host's current hardware profile. (shows the variance between current and default setups) Think of this as a redirection of the HKLM CurrentControlSet profile key. |
| HKEY_CLASSES_ROOT   | HKCR         | Filetype information, UI extensions, and backward compatibility settings are defined here.                                                                                                               |
| HKEY_CURRENT_USER   | HKCU         | Value entries here define the specific OS and software settings for each specific user. Roaming profile settings, including user preferences, are stored under HKCU.                                     |
| HKEY_USERS          | HKU          | The default User profile and current user configuration settings for the local computer are defined under HKU.                                                                                           |

There are other predefined keys for the Registry, but they are specific to certain versions and regional settings in Windows. For more information on those entries and Registry keys in general, check out the documentation provided by Microsoft.

## Why Is The Information Stored Within The Registry Important?

As a pentester, the Registry can be a treasure trove of information that can help us further our engagements. Everything from what software is installed, current OS revision, pertinent security settings, control of Defender, and more can be found in the Registry. Can we find all of this information in other places? Yes. But there is no better single point to find all of it and have the ability to make widespread changes to the host simultaneously. From an offensive perspective, the Registry is hard for Defenders to protect. The hives are enormous and filled with hundreds of entries. Finding a singular change or addition among the hives is like hunting for a needle in a haystack (unless they keep solid backups of their configurations and host states). Having a general understanding of the Registry and where key values are within can help us take action quicker and for defenders spot any issues sooner.

## How Do We Access the Information?

From the CLI, we have several options to access the Registry and manage our keys. The first is using `reg.exe`. Reg is a dos executable explicitly made for use in managing Registry settings. The second is using the `Get-Item` and `Get-ItemProperty` cmdlets to read keys and values. If we wish to make a change, the use of `New-ItemProperty` will do the trick.

### Querying Registry Entries

We will look at using `Get-Item` and `Get-ChildItem` first. Below we can see the output from using `Get-Item` and piping the result to `Select-Object`.

```powershell
PS C:\htb> Get-Item -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run | Select-Object -ExpandProperty Property

SecurityHealth
RtkAudUService
WavesSvc
DisplayLinkTrayApp
LogiOptions
Acrobat Assistant 8.0
(default)
Focusrite Notifier
AdobeGCInvoker-1.0
```

It's a simple output and only shows us the name of the services/applications currently running. If we wished to see each key and object within a hive, we could also use `Get-ChildItem` with the `-Recurse` parameter like so:

```powershell
PS C:\htb> Get-ChildItem -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Recurse

Hive: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths
<SNIP>
Name                           Property
----                           --------
7zFM.exe                       (default) : C:\Program Files\7-Zip\7zFM.exe
                               Path      : C:\Program Files\7-Zip\
Acrobat.exe                    (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrobat.exe
                               Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
AcrobatInfo.exe                (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\AcrobatInfo.exe
                               Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
AcroDist.exe                   Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
                               (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrodist.exe
Ahk2Exe.exe                    (default) : C:\Program Files\AutoHotkey\Compiler\Ahk2Exe.exe
AutoHotkey.exe                 (default) : C:\Program Files\AutoHotkey\AutoHotkey.exe
chrome.exe                     (default) : C:\Program Files\Google\Chrome\Application\chrome.exe
                               Path      : C:\Program Files\Google\Chrome\Application
<SNIP>
```

Now we snipped the output because it is expanding and showing each key and associated values within the `HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion` key. We can make our output easier to read using the `Get-ItemProperty` cmdlet. Let's try that same query but with `Get-ItemProperty`.

```powershell
PS C:\htb> Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

SecurityHealth        : C:\Windows\system32\SecurityHealthSystray.exe
RtkAudUService        : "C:\Windows\System32\DriverStore\FileRepository\realtekservice.inf_amd64_85cff5320735903
                        d\RtkAudUService64.exe" -background
WavesSvc              : "C:\Windows\System32\DriverStore\FileRepository\wavesapo9de.inf_amd64_d350b8504310bbf5\W
                        avesSvc64.exe" -Jack
DisplayLinkTrayApp    : "C:\Program Files\DisplayLink Core Software\DisplayLinkTrayApp.exe" -basicMode
LogiOptions           : C:\Program Files\Logitech\LogiOptions\LogiOptions.exe /noui
Acrobat Assistant 8.0 : "C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrotray.exe"
(default)             :
Focusrite Notifier    : "C:\Program Files\Focusriteusb\Focusrite Notifier.exe"
AdobeGCInvoker-1.0    : "C:\Program Files (x86)\Common Files\Adobe\AdobeGCClient\AGCInvokerUtility.exe"
PSPath                : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Curren
                        tVersion\Run
PSParentPath          : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Curren
                        tVersion
PSChildName           : Run
PSProvider            : Microsoft.PowerShell.Core\Registry
```

Now let's break this down. We issued the `Get-ItemProperty` command, specified out path as looking into the Registry, and specified the key `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`. The output provides us with the name of the services started and the value that was used to run them (the path they were executed from). This Registry key is used to start services/applications when a user logs in to the host. It is a great key to have visibility over and to keep in mind as a penetration tester. There are several versions of this key which we will discuss a little later in this section. Using `Get-ItemProperty` is much more readable than `Get-Item` was. When it comes to querying information, we can also use `Reg.exe`. Let's take a look at the output from that. We are going to look at a more straightforward key for this example.

```powershell
PS C:\htb> reg query HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip

HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip
    Path64    REG_SZ    C:\Program Files\7-Zip\
    Path    REG_SZ    C:\Program Files\7-Zip\
```

We queried the `HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip` key with `Reg.exe`, which provided us with the associated values. We can see that two values are set, `Path` and `Path64`, the `ValueType` is a `Reg_SZ` value which specifies that it contains a Unicode or ASCII string, and that value is the path to 7-Zip `C:\Program Files\7-Zip\`.

## Finding Info In The Registry

For us as pentesters and administrators, finding data within the Registry is a must-have skill. This is where `Reg.exe` really shines for us. We can use it to search for keywords and strings like `Password` and `Username` through key and value names or the data contained. Before we put it to use, let's break down the use of `Reg Query`. We will look at the command string `REG QUERY HKCU /F "password" /t REG_SZ /S /K`.

- `Reg query`: We are calling on `Reg.exe` and specifying that we want to query data.
- `HKCU`: This portion is setting the path to search. In this instance, we are looking in all of `HKey_Current_User`.
- `/f "password"`: `/f` sets the pattern we are searching for. In this instance, we are looking for "Password".
- `/t REG_SZ`: `/t` is setting the value type to search. If we do not specify, `reg query` will search through every type.
- `/s`: `/s` says to search through all subkeys and values recursively.
- `/k`: `/k` narrows it down to only searching through Key names.

### Searching With Reg Query

```powershell
PS C:\htb>  REG QUERY HKCU /F "Password" /t REG_SZ /S /K

HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\Winlogon\PasswordExpiryNotification
    NotShownErrorTime    REG_SZ    08::23::24, 2022/10/19
    NotShownErrorReason    REG_SZ    GetPwdResetInfoFailed

End of search: 2 match(es) found.
```

Our results from this query could be more exciting, but it's still worth taking a look and using a similar search for other keywords and phrases like `Username`, `Credentials`, and `Keys`. We could be surprised by what we find. As we can see, querying registry keys and values is relatively easy. What if we want to set a new value or create a new key?

## Creating and Modifying Registry Keys and Values

When dealing with the modification or creation of new keys and values, we can use standard PowerShell cmdlets like `New-Item`, `Set-Item`, `New-ItemProperty`, and `Set-ItemProperty` or utilize `Reg.exe` again to make the changes we need. Let's try and create a new Registry Key below. For our example, we will create a new test key in the `RunOnce` hive `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` named `TestKey`. By placing the key and value in `RunOnce`, after it executes, it will be deleted.

### Scenario:

We have landed on a host and can add a new registry key for persistence. We need to set a key named `TestKey` and a value of `C:\Users\htb-student\Downloads\payload.exe` that tells `RunOnce` to run our payload we leave on the host the next time the user logs in. This will ensure that if the host restarts or we lose access, the next time the user logs in, we will get a new shell.

```powershell
# Create New Registry Key
PS C:\htb> New-Item -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\ -Name TestKey

    Hive: HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce

Name                           Property
----                           --------
TestKey
```

```powershell
# Set New Registry Item Property
PS C:\htb>  New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey -Name  "access" -PropertyType String -Value "C:\Users\htb-student\Downloads\payload.exe"

access       : C:\Users\htb-student\Downloads\payload.exe
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\
               TestKey
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
PSChildName  : TestKey
PSDrive      : HKCU
PSProvider   : Microsoft.PowerShell.Core\Registry
```

If we wanted to add the same key/value pair using `Reg.exe`, we would do so like this:

```powershell
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce\TestKey" /v access /t REG_SZ /d "C:\Users\htb-student\Downloads\payload.exe"
```

Now in a real pentest, we would have left an executable payload on the host, and in the instance that the host reboots or the user logs in, we would acquire a new shell to our C2. This value doesn't do much for us right now, so let's practice deleting it.

```powershell
# Delete Reg properties
PS C:\htb> Remove-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey -Name  "access"

PS C:\htb> Get-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey
```

If no error window popped up, our key/value pair was deleted successfully. However, this is one of those things you should be extremely careful with. Removing entries from the Windows Registry could negatively affect the host and how it functions. Be sure you know what it is you are removing before. In the wise words of Uncle Ben, "With great power comes great responsibility."

## Onwards

Now that we have Registry management down, it's time to move on to handling Event Logs through PowerShell.
