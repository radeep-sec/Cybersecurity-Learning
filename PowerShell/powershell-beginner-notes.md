# PowerShell Notes – Beginner to Cybersecurity

## 1. Introduction to PowerShell

### What is PowerShell?

PowerShell is a command-line shell, scripting language, and task automation framework developed by Microsoft.

It allows users to interact with and manage computer systems by entering commands or writing scripts.

With PowerShell, you can:
- Manage files and folders.
- Monitor running processes and services.
- Retrieve computer and network information.
- Manage users and system configurations.
- Automate repetitive tasks.
- Execute commands on remote computers.
- Investigate suspicious activity on a system.

PowerShell is particularly important in cybersecurity because it provides access to many Windows system functions and can automate security-related tasks.

### A brief history

- PowerShell was originally released in 2006.
- It was initially designed for Windows.
- Jeffrey Snover was a key person behind its development and object-oriented design.
- PowerShell Core was introduced in 2016, bringing cross-platform support for Windows, Linux and macOS.
- Windows PowerShell 5.1 and modern PowerShell (PowerShell 7 and later) are different editions.

**Important:** Windows PowerShell 5.1 is the older Windows-focused edition. Modern PowerShell is cross-platform.

---

## 2. Understanding Cmdlets

### What is a cmdlet?

A cmdlet (pronounced command-let) is a specialised command used in PowerShell to perform a particular task.

For example:

```powershell
Get-Process
```

This command retrieves information about processes currently running on the computer.

PowerShell cmdlets generally follow a standard naming convention:

**Verb-Noun**

Examples:

| Cmdlet | Purpose |
|---|---|
| Get-Process | Retrieves running processes |
| Get-Service | Retrieves system services |
| Get-ChildItem | Lists files and folders |
| Set-Location | Changes the current directory |
| New-Item | Creates a file or folder |
| Remove-Item | Deletes an item |
| Copy-Item | Copies an item |
| Move-Item | Moves an item |

The verb describes the action, while the noun describes the object or resource being acted upon.

For example:
- Get = retrieve information.
- Process = the resource being inspected.

### Finding available commands

PowerShell provides `Get-Command` to discover commands.

Example:

```powershell
Get-Command
```

This lists available commands.

To find commands using a particular verb:

```powershell
Get-Command -Verb Remove
```

This lists commands whose verb is Remove.

### Getting help

When you encounter an unfamiliar cmdlet, use `Get-Help`.

```powershell
Get-Help Get-Process
```

To see more detailed information:

```powershell
Get-Help Get-Process -Full
```

To see practical examples:

```powershell
Get-Help Invoke-Command -Examples
```

**My learning tip:** You don't need to memorise every PowerShell command. Learn how to find commands and read their help documentation.

---

## 3. Parameters and Arguments

A parameter provides additional information to a cmdlet about what it should do or what resource it should work with.

For example:

```powershell
Get-ChildItem -Path C:\Users
```

Here:
- `Get-ChildItem` is the cmdlet.
- `-Path` is the parameter.
- `C:\Users` is the argument (the value supplied to the parameter).

Some parameters can be omitted when PowerShell supports positional arguments.

For example, these commands can produce the same result:

```powershell
Get-ChildItem -Path C:\Users
```

```powershell
Get-ChildItem C:\Users
```

The second command works because PowerShell can interpret the first positional argument as the path.

However, using named parameters often makes commands easier to understand.

**Remember:**
- Cmdlet = the instruction.
- Parameter = what additional information the cmdlet needs.
- Argument = the value supplied to a parameter.

---

## 4. Modules and Repositories

### What is a PowerShell module?

A module is a package containing related PowerShell functionality.

Modules can provide additional cmdlets, functions and other resources.

For example, the Az module provides PowerShell tools for managing Microsoft Azure resources.

### What is a repository?

A repository is a location where software packages and modules are stored and made available for installation.

The PowerShell Gallery is Microsoft's public repository for PowerShell modules and scripts.

### Finding a module

Use `Find-Module` to search for a module.

```powershell
Find-Module -Name Az
```

To search using a wildcard:

```powershell
Find-Module -Name Az*
```

The `*` wildcard represents any number of characters.

### Installing a module

Use `Install-Module` to install a module.

```powershell
Install-Module -Name Az
```

**Important distinction:**
- `Find-Module` searches for a module.
- `Install-Module` installs a module.

These commands may require internet access and appropriate permissions. They might not work in restricted TryHackMe lab environments.

---

# 5. Navigating the File System

PowerShell can be used to explore, create, read, copy, move and delete files and directories.

### Understanding the PowerShell prompt

Example:

```powershell
PS C:\Users\captain>
```

This tells us:
- `PS` means we are using PowerShell.
- `C:\Users\captain` is the current working directory.
- `>` indicates that PowerShell is ready to receive a command.

The current working directory is the location PowerShell treats as the starting point for relative paths.

### Understanding paths

A path tells PowerShell where a file or folder is located.

There are two main types:

**Absolute path**

An absolute path specifies the complete location.

Example:

```powershell
C:\Users\captain\Documents
```

**Relative path**

A relative path describes a location in relation to the current directory.

Important symbols:

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `..` | Parent directory |
| `.\` | A location relative to the current directory |
| `..\` | A location relative to the parent directory |

For example, if the current directory is `C:\Users\captain`:

```powershell
.\Documents
```

Refers to:

```text
C:\Users\captain\Documents
```

### Get-ChildItem

`Get-ChildItem` lists files and directories inside a location.

```powershell
Get-ChildItem
```

Lists the contents of the current directory.

To inspect a particular location:

```powershell
Get-ChildItem -Path C:\Users
```

You can also use:

```powershell
Get-ChildItem .\Documents
```

This displays the contents of Documents without changing your current directory.

Useful parameters:

```powershell
Get-ChildItem -Directory
```

Displays directories only.

```powershell
Get-ChildItem -File
```

Displays files only.

```powershell
Get-ChildItem -Recurse
```

Searches through the current directory and its subdirectories.

Example:

```powershell
Get-ChildItem -Path C:\Users\captain -Directory -Recurse -Filter SecretFiles
```

Searches for directories named SecretFiles underneath the specified location.

### Understanding Get-ChildItem output

A typical listing includes:

| Column | Meaning |
|---|---|
| Mode | File or directory attributes |
| LastWriteTime | Date and time the item was last modified |
| Length | File size in bytes |
| Name | Name of the file or directory |

A `d` at the beginning of the Mode column indicates a directory. A `-` generally indicates a file.

### Set-Location

`Set-Location` changes the current working directory.

Example:

```powershell
Set-Location -Path C:\Users
```

You can also use its common alias:

```powershell
cd C:\Users
```

To move into a subdirectory:

```powershell
cd .\Documents
```

To move up one directory:

```powershell
cd ..
```

**Important:** `Get-ChildItem` explores a location, while `Set-Location` actually moves you into that location.

### New-Item

`New-Item` creates a new file or directory.

Create a directory:

```powershell
New-Item -Path .\Projects -ItemType Directory
```

Create a file:

```powershell
New-Item -Path .\notes.txt -ItemType File
```

Here:
- `-Path` specifies where the item should be created.
- `-ItemType` specifies what kind of item to create.

### Remove-Item

`Remove-Item` deletes files and directories.

```powershell
Remove-Item -Path .\notes.txt
```

To remove a directory and its contents:

```powershell
Remove-Item -Path .\Projects -Recurse
```

The `-Recurse` parameter allows removal of items inside the directory as well.

**Security reminder:** Be careful when using `Remove-Item`, particularly with recursive deletion. It can permanently remove important data.

### Copy-Item

`Copy-Item` creates a copy of a file or directory.

```powershell
Copy-Item -Path .\notes.txt -Destination .\backup.txt
```

The original file remains in its original location.

### Move-Item

`Move-Item` transfers a file or directory to another location.

```powershell
Move-Item -Path .\notes.txt -Destination .\Documents
```

Unlike copying, moving means the original item is no longer at its previous location.

### Get-Content

`Get-Content` reads the contents of a file.

```powershell
Get-Content -Path .\notes.txt
```

It displays the text stored inside the file.

An important distinction:

- `Get-ChildItem` displays information about files and folders.
- `Get-Content` displays the actual contents of a file.

For example, `Get-ChildItem` might tell you that `passwords.txt` exists and show its size. `Get-Content` attempts to read the text inside it.

---

# 6. Working with Objects and Properties

One of the most important PowerShell concepts is its use of objects.

Unlike traditional command-line tools that often produce plain text, PowerShell cmdlets commonly return structured objects.

### What is an object?

An object is a structured collection of information representing something.

For example, when you run:

```powershell
Get-Process
```

PowerShell returns process objects containing information about running programs.

Each process object can have properties such as:
- Name
- Id
- CPU
- Handles
- Working set memory

### What is a property?

A property is a piece of information belonging to an object.

For example:

- Object: A running process.
- Property: Its name.
- Property: Its process ID.
- Property: Its CPU usage.

### What is a method?

A method is an action that an object can perform.

The distinction is:

- Properties describe an object.
- Methods perform actions associated with an object.

### Understanding Get-Member

`Get-Member` helps you inspect the properties and methods available on objects.

Example:

```powershell
Get-Location | Get-Member
```

This displays information about the members of the object returned by `Get-Location`.

To retrieve a particular property:

```powershell
(Get-Location).Path
```

This displays the current location's path.

### Accessing properties

Suppose you want to display only process names and IDs:

```powershell
Get-Process | Select-Object Name, Id
```

This selects the Name and Id properties from the process objects.

**Important:** A cmdlet is an instruction, not the object itself. The object is the structured result returned by the cmdlet.

---

# 7. The PowerShell Pipeline

The pipeline is one of PowerShell's most powerful features.

The pipe symbol is:

```text
|
```

It passes the output of one command to another command.

This allows you to combine commands to perform more complex tasks.

### Example

```powershell
Get-Process | Sort-Object CPU
```

This works in two stages:

1. `Get-Process` retrieves running process objects.
2. `Sort-Object CPU` sorts those objects according to their CPU property.

The output of the first cmdlet becomes the input of the second.

Think of the pipeline as a production line: each command receives information, processes it, and passes it along.

### Sorting objects with Sort-Object

Sort processes by CPU usage:

```powershell
Get-Process | Sort-Object CPU
```

Sort from highest to lowest:

```powershell
Get-Process | Sort-Object CPU -Descending
```

Sort files by size:

```powershell
Get-ChildItem -File | Sort-Object Length
```

### Selecting information with Select-Object

`Select-Object` allows you to choose which properties to display or how many objects to return.

Example:

```powershell
Get-Process | Select-Object Name, Id
```

Displays only process names and IDs.

To return only the first object:

```powershell
Get-Process | Select-Object -First 1
```

### Filtering with Where-Object

`Where-Object` filters objects based on a condition.

Example:

```powershell
Get-ChildItem | Where-Object -Property Extension -eq ".txt"
```

This returns files whose Extension property equals `.txt`.

The difference between filtering and selecting is important:

- `Where-Object` decides which objects should remain.
- `Select-Object` decides which properties or how many objects should be displayed.

### Useful comparison operators

| Operator | Meaning | Example |
|---|---|---|
| `-eq` | Equal to | `Name -eq "notepad"` |
| `-ne` | Not equal to | `Name -ne "notepad"` |
| `-gt` | Greater than | `Length -gt 1000` |
| `-ge` | Greater than or equal to | `Length -ge 1000` |
| `-lt` | Less than | `Length -lt 1000` |
| `-le` | Less than or equal to | `Length -le 1000` |
| `-like` | Matches a wildcard pattern | `Name -like "ship*"` |

### Understanding wildcards

The `*` symbol represents zero or more characters.

For example:

```powershell
Get-ChildItem | Where-Object -Property Name -like "ship*"
```

This matches names beginning with `ship`.

Examples of matching names:
- ship.txt
- ship-log.txt
- shipping-records

### Practical pipeline example: finding the largest file

```powershell
Get-ChildItem -File | Sort-Object Length -Descending | Select-Object -First 1
```

Breaking it down:

1. `Get-ChildItem -File` retrieves files only.
2. `Sort-Object Length -Descending` sorts files from largest to smallest.
3. `Select-Object -First 1` returns the largest file.

This is a good example of combining several small commands into one useful operation.

---

# 8. Searching Inside Files with Select-String

`Select-String` searches for specific text or patterns inside files.

Example:

```powershell
Select-String -Path .\captain-hat.txt -Pattern "hat"
```

This searches the file for occurrences of the word `hat`.

The output can include:
- File name.
- Line number.
- Matching line.

This is particularly useful when analysing log files, scripts and configuration files.

### Understanding regular expressions

Regular expressions (regex) are patterns used to search for specific text.

Examples:

| Pattern | Meaning |
|---|---|
| `^hat` | Matches text beginning with hat |
| `hat$` | Matches text ending with hat |
| `^[0-9]+$` | Matches a line containing only one or more digits |

Explanation of the last example:
- `^` means beginning of the line.
- `[0-9]` means any digit from 0 to 9.
- `+` means one or more occurrences.
- `$` means end of the line.

Regex is useful when searching large files for patterns rather than one exact word.

---

# 9. Gathering System Information

PowerShell provides cmdlets for inspecting the computer's operating system, hardware, users and network configuration.

These commands are especially useful during system administration and security investigations.

### Get-ComputerInfo

Retrieves information about the computer's operating system and hardware.

```powershell
Get-ComputerInfo
```

It can provide information such as:
- Operating system name.
- Windows edition.
- Windows version.
- Installation date.
- BIOS information.
- Hardware information.

Useful properties include:

| Property | Information |
|---|---|
| WindowsProductName | Windows product name |
| WindowsEditionId | Windows edition identifier |
| WindowsInstallationType | Type of Windows installation |
| WindowsInstallDateFromRegistry | Installation date recorded in the registry |
| WindowsCurrentVersion | Windows version information |
| WindowsBuildLabEx | Windows build information |

### Get-LocalUser

Lists local user accounts on the computer.

```powershell
Get-LocalUser
```

The output can include:
- Name
- Enabled
- Description

The Enabled property indicates whether an account is enabled.

- True = account is enabled.
- False = account is disabled.

**Cybersecurity relevance:** Reviewing user accounts can help identify unexpected accounts, suspicious account descriptions or accounts that should no longer be active.

Note that local accounts are not necessarily the same as domain accounts.

---

# 10. Network Information and Configuration

Understanding network configuration is essential in cybersecurity.

PowerShell provides several cmdlets for inspecting IP addresses, network interfaces, gateways and DNS settings.

### Important networking terminology

**IP address**

An IP address identifies a network interface on an IP network.

Example:

```text
10.10.178.209
```

**Network interface**

A network interface is the connection through which a device communicates with a network.

Examples:
- Ethernet
- Wi-Fi

**Default gateway**

A default gateway is usually the router or network device that forwards traffic from your local network to other networks.

**DNS server**

DNS stands for Domain Name System.

A DNS server translates domain names into IP addresses.

For example, it helps a computer find the IP address associated with a website name.

### Get-NetIPConfiguration

Displays an overview of network configuration.

```powershell
Get-NetIPConfiguration
```

It can show:
- Network interface information.
- IP addresses.
- Default gateway.
- DNS server addresses.

This is useful when you want a general overview of how the computer is connected to the network.

### Get-NetIPAddress

Provides detailed information about IP addresses assigned to network interfaces.

```powershell
Get-NetIPAddress
```

Useful properties include:

| Property | Meaning |
|---|---|
| IPAddress | IP address assigned to the interface |
| InterfaceAlias | Name of the network interface |
| AddressFamily | IPv4 or IPv6 |
| PrefixLength | Network prefix length |
| AddressState | State of the IP address |
| PreferredLifetime | Remaining preferred lifetime of the address |

### IPv4 and IPv6

There are two commonly used versions of IP addressing.

**IPv4 example:**

```text
10.10.178.209
```

**IPv6 example:**

```text
fe80::3fef:360c:304:64e%5
```

IPv6 addresses beginning with `fe80::` are link-local addresses. They are used for communication on the local network link.

### Loopback addresses

Loopback addresses refer to the local computer itself.

Examples:

```text
127.0.0.1
```

IPv4 loopback address.

```text
::1
```

IPv6 loopback address.

### Difference between the two network cmdlets

| Cmdlet | Main purpose |
|---|---|
| Get-NetIPConfiguration | Provides a general network configuration overview |
| Get-NetIPAddress | Displays detailed information about assigned IP addresses |

---

# 11. Monitoring Processes and Services

Processes and services are important parts of a Windows system.

Monitoring them can help security professionals identify unusual activity.

### Get-Process

Lists processes currently running on the computer.

```powershell
Get-Process
```

A process is a running instance of a program.

For example, when you open Notepad, Windows creates a process for it.

The output may include:

| Column | Meaning |
|---|---|
| Handles | Number of object handles held by the process |
| NPM(K) | Non-paged memory usage in KB |
| PM(K) | Paged memory usage in KB |
| WS(K) | Working set memory in KB |
| CPU(s) | Accumulated CPU time in seconds |
| Id | Process ID |
| SI | Session ID |
| ProcessName | Name of the process |

### What is a PID?

PID stands for Process Identifier.

It is a numerical identifier assigned to a running process.

Example:

```powershell
Get-Process -Id 516
```

This retrieves information about the process with PID 516.

A PID helps identify a particular running process.

### Get-Service

Lists Windows services.

```powershell
Get-Service
```

A service is a background program or system component that performs a particular function.

Examples include:
- Windows Update.
- Background network services.
- Security-related services.

The output includes:

| Column | Meaning |
|---|---|
| Status | Current service state |
| Name | Service name |
| DisplayName | Human-readable service name |

Common service statuses:

- Running = service is currently running.
- Stopped = service is not running.
- Paused = service is temporarily paused.

### Difference between a process and a service

A process is a running instance of a program.

A service is a background component designed to provide a system or application function.

Services generally run within processes, but not every process is a service. Multiple services can also share a process.

**Cybersecurity relevance:** Unusual processes or unexpected services can be clues during an investigation. However, an unfamiliar process or service is not automatically malicious.

---

# 12. Inspecting Network Connections with Get-NetTCPConnection

`Get-NetTCPConnection` displays TCP network connection information.

```powershell
Get-NetTCPConnection
```

TCP stands for Transmission Control Protocol.

TCP is a network protocol that establishes connections between devices and helps deliver data reliably.

### Understanding ports

A port is a logical endpoint used by applications and services to communicate over a network.

Common ports include:

| Port | Common use |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | Remote Desktop Protocol (RDP) |
| 22 | SSH |

These are conventional ports; applications can use different ports depending on their configuration.

### Important Get-NetTCPConnection properties

| Property | Meaning |
|---|---|
| LocalAddress | Local IP address |
| LocalPort | Local port |
| RemoteAddress | Remote IP address |
| RemotePort | Remote port |
| State | Current TCP connection state |
| OwningProcess | PID of the process associated with the connection |

To display selected properties:

```powershell
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, OwningProcess
```

### Understanding TCP connection states

**Listen**

The computer is waiting for an incoming TCP connection on a particular port.

**Established**

A TCP connection has been established and is active.

**TimeWait**

A connection has recently closed, but the system keeps its state temporarily before fully releasing it.

**CloseWait**

The remote side has closed its connection, but the local application has not yet closed its side.

### Understanding 0.0.0.0

When you see:

```text
0.0.0.0
```

as a local listening address, it generally means the service is listening on all available local IPv4 interfaces.

It does not mean that the computer has no IP address.

### Identifying which process owns a connection

One particularly important property is:

```text
OwningProcess
```

This contains the PID of the process associated with the TCP connection.

To display it:

```powershell
Get-NetTCPConnection | Select-Object OwningProcess
```

To identify the actual process:

```powershell
Get-Process -Id 1234
```

Replace `1234` with the PID obtained from `OwningProcess`.

You can also combine the commands:

```powershell
Get-NetTCPConnection | ForEach-Object {
    Get-Process -Id $_.OwningProcess -ErrorAction SilentlyContinue
}
```

This retrieves process information for the processes associated with the connections. The same process may appear more than once if it owns multiple connections.

**Cybersecurity relevance:** Monitoring network connections can help identify unexpected outbound communication, suspicious listening ports and processes communicating with unfamiliar remote addresses.

An unfamiliar connection is a clue to investigate, not proof of an attack.

---

# 13. File Integrity and Hashing

### Get-FileHash

`Get-FileHash` calculates a cryptographic hash of a file.

Example:

```powershell
Get-FileHash -Path .\ship-flag.txt
```

A hash is a fixed-length digital fingerprint calculated from the contents of a file.

PowerShell uses SHA256 by default.

Example output:

```text
Algorithm : SHA256
Hash      : [calculated hash value]
Path      : C:\Users\captain\ship-flag.txt
```

### What is SHA256?

SHA256 is a cryptographic hashing algorithm that produces a 256-bit hash value.

It is commonly used to verify file integrity.

If even a small change is made to a file, its hash will generally change significantly.

### Why is hashing important in cybersecurity?

Hashing can be used to:
- Verify that a file has not changed.
- Compare files against known malware hashes.
- Check file integrity.
- Identify files during digital forensic investigations.

**Important distinction:**

Hashing is not encryption.

- Encryption is designed to protect information so it can be decrypted with the appropriate key.
- Hashing produces a one-way fingerprint and is not designed to be reversed.

---

# 14. Alternate Data Streams (ADS)

Alternate Data Streams are a feature of the NTFS file system used by Windows.

They allow additional data to be associated with an existing file.

A normal file has a default data stream containing its usual contents. NTFS can also store additional named streams associated with that file.

For example, an additional stream could be named `housinginfo` and attached to:

```text
house_log.txt
```

The additional stream is not normally displayed as a separate file in standard File Explorer views.

### Inspecting Alternate Data Streams

Use:

```powershell
Get-Item -Path C:\House\house_log.txt -Stream *
```

This displays the streams associated with the file.

The `*` wildcard means all matching streams.

### Why are ADS important in cybersecurity?

ADS can be used legitimately by Windows and applications, but they can also be abused to store hidden information.

For example, an attacker might attempt to hide data inside an alternate stream so that it is less obvious during a basic file inspection.

**Important:** The presence of ADS is not automatically evidence of malicious activity. It is something worth investigating when analysing a suspicious file.

ADS is a feature of NTFS, so it is not available in the same way on every file system.

---

# 15. PowerShell Scripting

### What is scripting?

Scripting is the process of writing and executing a sequence of commands stored in a text file.

The file containing those commands is called a script.

In PowerShell, script files normally use the `.ps1` extension.

For example:

```text
system-check.ps1
```

A script is essentially a to-do list for the computer.

Instead of entering commands individually every time, you can save them in a file and execute them together.

### Example of a simple script

Imagine you want to collect information about processes, services and network configuration.

You could create a file called:

```text
system-check.ps1
```

And place these commands inside:

```powershell
Get-Process

Get-Service

Get-NetIPConfiguration
```

When the script runs, PowerShell executes the commands in sequence.

### Why is scripting useful?

Scripting helps to:
- Automate repetitive tasks.
- Reduce manual effort.
- Make tasks more consistent.
- Process large amounts of information.
- Perform complex operations more efficiently.

### Scripting in blue team cybersecurity

Blue team professionals focus on defending systems and investigating security incidents.

They can use PowerShell scripts to:
- Analyse system logs.
- Detect unusual processes.
- Monitor network connections.
- Extract indicators of compromise (IOCs).
- Check file integrity.
- Automate security investigations.

An IOC is an indicator of compromise: information that may help identify malicious activity, such as a suspicious file hash, IP address or domain.

### Scripting in red team cybersecurity

Red team professionals simulate attacks to test security.

They can use PowerShell scripts to:
- Enumerate systems.
- Gather system information.
- Automate authorised testing activities.
- Execute commands remotely.
- Test security controls.

Scripts can also be obfuscated to make their contents harder to understand or detect. This technique can be abused by attackers, so it is important to understand it from a defensive perspective too.

### Scripting for system administrators

System administrators use scripts to:
- Manage user accounts.
- Check system health.
- Automate configuration tasks.
- Enforce security policies.
- Monitor computers and networks.
- Perform routine maintenance.

---

# 16. Remote Command Execution with Invoke-Command

### What is Invoke-Command?

`Invoke-Command` is a PowerShell cmdlet used to execute commands on local or remote computers.

Remote execution means running commands on another computer through a network connection.

This is useful when administrators need to manage multiple systems without physically accessing each one.

### Example 1: Running a script on a remote computer

```powershell
Invoke-Command -FilePath C:\scripts\test.ps1 -ComputerName Server01
```

Explanation:

| Part | Meaning |
|---|---|
| Invoke-Command | Executes commands locally or remotely |
| -FilePath | Specifies the script file |
| C:\scripts\test.ps1 | Location of the script on the local computer |
| -ComputerName | Specifies the remote computer |
| Server01 | Name of the remote computer |

In simple terms:

PowerShell takes the script stored on your local computer, runs it on Server01 and returns the results.

The script file is specified from the local computer, but its commands execute on the remote computer.

### Example 2: Running a command remotely

```powershell
Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Culture }
```

Explanation:

| Part | Meaning |
|---|---|
| Invoke-Command | Runs the command remotely |
| -ComputerName | Specifies the remote computer |
| Server01 | Name of the remote computer |
| -Credential | Specifies the account used for authentication |
| Domain01\User01 | Domain and username |
| -ScriptBlock | Specifies the code to execute |
| { Get-Culture } | The command being executed |

PowerShell may prompt for the account password.

The account must have the necessary permissions to execute commands remotely.

### What is a ScriptBlock?

A ScriptBlock is a piece of PowerShell code enclosed in curly braces.

Example:

```powershell
{ Get-Process }
```

It represents code that PowerShell can execute.

A ScriptBlock can also contain multiple commands:

```powershell
{
    Get-Process
    Get-Service
    Get-NetIPConfiguration
}
```

### Example 3: Executing Get-Service remotely

Suppose you want to execute `Get-Service` on a remote computer named `RoyalFortune`, and no additional credentials are required.

```powershell
Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }
```

This executes `Get-Service` on RoyalFortune and returns the results to your local PowerShell session.

### Why is Invoke-Command important in cybersecurity?

**Blue team uses:**
- Collecting information from remote computers.
- Checking running services across an organisation.
- Running security checks on multiple systems.
- Automating incident response.

**Red team uses:**
- Executing authorised commands on target systems.
- Performing remote system enumeration.
- Testing remote access and security controls.

Remote execution is powerful, but it requires appropriate permissions and a properly configured remote management connection. It should only be used on systems you are authorised to access.

---

# 17. Important PowerShell Concepts – Quick Revision

| Concept | Explanation |
|---|---|
| PowerShell | Command-line shell and scripting language |
| Cmdlet | Specialised PowerShell command |
| Object | Structured information returned by a command |
| Property | Information belonging to an object |
| Method | Action an object can perform |
| Parameter | An option that changes how a cmdlet operates |
| Argument | Value supplied to a parameter |
| Module | Package containing PowerShell functionality |
| Repository | Location where modules and packages are stored |
| Pipeline | Passes output from one cmdlet to another |
| Wildcard | Character used to match patterns |
| Regex | Pattern language used to search text |
| Script | File containing commands to execute |
| ScriptBlock | PowerShell code enclosed in curly braces |
| PID | Process Identifier |
| TCP | Transmission Control Protocol |
| Port | Logical communication endpoint |
| IP address | Address identifying a network interface |
| DNS | System that translates domain names into IP addresses |
| Hash | Digital fingerprint of data |
| ADS | Additional named data streams associated with NTFS files |
| Remote execution | Running commands on another computer |

---

# 18. Essential Commands to Remember

This is a quick-reference list of the commands covered in these notes.

### Help and discovery

```powershell
Get-Command
Get-Command -Verb Remove
Get-Help Get-Process
Get-Help Get-Process -Full
Get-Help Invoke-Command -Examples
Get-Member
```

### Modules

```powershell
Find-Module -Name Az
Find-Module -Name Az*
Install-Module -Name Az
```

### File system

```powershell
Get-ChildItem
Get-ChildItem -Path C:\Users
Get-ChildItem -File
Get-ChildItem -Directory
Get-ChildItem -Recurse
Set-Location C:\Users
New-Item -Path .\notes.txt -ItemType File
New-Item -Path .\Projects -ItemType Directory
Remove-Item -Path .\notes.txt
Copy-Item -Path .\notes.txt -Destination .\backup.txt
Move-Item -Path .\notes.txt -Destination .\Documents
Get-Content -Path .\notes.txt
```

### Objects and pipelines

```powershell
Get-Location | Get-Member
(Get-Location).Path
Get-Process | Select-Object Name, Id
Get-Process | Sort-Object CPU -Descending
Get-ChildItem | Where-Object -Property Extension -eq ".txt"
Get-Process | Select-Object -First 1
```

### Searching

```powershell
Select-String -Path .\notes.txt -Pattern "password"
```

### System information

```powershell
Get-ComputerInfo
Get-LocalUser
```

### Networking

```powershell
Get-NetIPConfiguration
Get-NetIPAddress
Get-NetTCPConnection
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, OwningProcess
```

### Processes and services

```powershell
Get-Process
Get-Process -Id 516
Get-Service
```

### File integrity and ADS

```powershell
Get-FileHash -Path .\notes.txt
Get-Item -Path C:\House\house_log.txt -Stream *
```

### Scripting and remote execution

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Service }
```

```powershell
Invoke-Command -FilePath C:\scripts\test.ps1 -ComputerName Server01
```

---

# 19. Final Thoughts: Why PowerShell Matters in Cybersecurity

PowerShell is much more than a way to type commands into a terminal. It is a powerful tool for understanding, managing and investigating computer systems.

The commands covered in these notes provide a foundation for several important cybersecurity activities:

- **System enumeration:** Discovering users, processes, services and system information.
- **Network investigation:** Inspecting IP addresses, ports and active connections.
- **File investigation:** Searching files, inspecting their contents and checking hashes.
- **Automation:** Using scripts to perform repetitive tasks.
- **Remote administration:** Executing commands on other computers.

As a beginner, the most important thing is not memorising every command. It is understanding what each command does, what information it returns, and how you can combine commands to solve problems.

Keep practising, experiment in your lab environment, and document what you learn.

**The goal is not just to know PowerShell commands. It is to understand how to use PowerShell to investigate and manage systems.**
