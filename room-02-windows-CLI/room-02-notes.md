# Room 2 - PowerShell: Detailed Notes

**TryHackMe - Windows Command Line Series**  
**Room 2: PowerShell Fundamentals**

---

## Task 1: Introduction to PowerShell

### Overview
PowerShell is Microsoft's modern task automation and configuration management framework. Unlike traditional command-line shells that process text, PowerShell operates on .NET objects, making it significantly more powerful for system administration and automation tasks.

### What Makes PowerShell Different?

**Object-Oriented Architecture:**
- Commands (cmdlets) output structured objects, not plain text
- Objects have properties and methods you can access
- Enables powerful filtering, sorting, and manipulation
- No need for complex text parsing

**Example:**
```powershell
# cmd.exe outputs text:
C:\> dir
Volume in drive C is Windows
...

# PowerShell outputs objects:
PS C:\> Get-ChildItem
Mode    LastWriteTime    Length Name
----    -------------    ------ ----
d-----  2/11/2026        ...    Documents
-a----  2/11/2026   1024 ...    file.txt
```

### Prerequisites
Before starting this room, you should understand:
- Basic Windows Command Prompt (cmd.exe) navigation (covered in Room 1)
- File system concepts (directories, paths, files)
- Basic networking concepts (IP addresses, DNS, ports)

---

## Task 2: What is PowerShell?

### Definition
**PowerShell** is both a command-line shell AND a scripting language built on Microsoft's .NET Framework. It's designed specifically for system administration and automation.

### PowerShell Evolution

**Windows PowerShell (Legacy):**
- First released in 2006 (PowerShell 1.0)
- Built on .NET Framework
- Windows-only
- Current version: 5.1 (maintenance mode)
- Pre-installed on all modern Windows systems

**PowerShell Core (Modern):**
- Released in 2016 (PowerShell Core 6.0)
- Built on .NET Core (cross-platform)
- Works on Windows, Linux, and macOS
- Current version: 7.x
- Open source on GitHub
- Recommended for new projects

### Key Features

**1. Cmdlet-Based Architecture:**
- Commands follow Verb-Noun naming (e.g., Get-Process, Set-Location)
- Consistent and predictable
- Easy to discover and learn

**2. Pipeline Support:**
- Pass objects between commands
- More powerful than text-based pipes
- Enables complex operations with simple syntax

**3. .NET Integration:**
- Full access to .NET libraries
- Create complex automation scripts
- Extensible with custom modules

**4. Remote Execution:**
- Manage multiple systems from one console
- PowerShell Remoting (PSRemoting)
- Essential for enterprise administration

### Use Cases for System Administrators

- **Server Management:** Configure and monitor Windows Servers remotely
- **Active Directory:** User/group management, security audits
- **Azure/Microsoft 365:** Cloud resource management
- **Automation:** Scheduled tasks, batch operations, configuration deployment
- **Troubleshooting:** Event log analysis, performance monitoring
- **Reporting:** Generate system status reports automatically

---

## Task 3: PowerShell Basics

### Launching PowerShell

**Method 1 - Start Menu Search:**
```
1. Press Windows key
2. Type "PowerShell"
3. Select "Windows PowerShell" or "PowerShell 7"
```

**Method 2 - Windows Terminal:**
```
1. Open Windows Terminal
2. Click dropdown menu
3. Select "PowerShell" profile
```

**Method 3 - Run Dialog:**
```
1. Press Win+R
2. Type "powershell"
3. Press Enter
```

**Method 4 - File Explorer:**
```
1. Navigate to any folder
2. Type "powershell" in address bar
3. Press Enter (opens PowerShell in that location)
```

### PowerShell Prompt Structure
```powershell
PS C:\Users\Username>
│  │                 │
│  │                 └─ Ready for command input
│  └─ Current working directory
└─ PowerShell indicator
```

### Cmdlet Naming Convention: Verb-Noun

PowerShell uses consistent Verb-Noun naming for all cmdlets:

**Common Verbs:**
- **Get** - Retrieve information (Get-Process, Get-Service)
- **Set** - Modify settings (Set-Location, Set-ExecutionPolicy)
- **New** - Create new items (New-Item, New-LocalUser)
- **Remove** - Delete items (Remove-Item, Remove-Service)
- **Start** - Begin operations (Start-Service, Start-Process)
- **Stop** - End operations (Stop-Service, Stop-Process)
- **Enable** - Activate features (Enable-WindowsOptionalFeature)
- **Disable** - Deactivate features (Disable-NetAdapter)

### Essential Basic Cmdlets

#### **Get-Command** - Discover Available Cmdlets
```powershell
# List ALL available cmdlets
Get-Command

# Find cmdlets with specific verb
Get-Command -Verb Get

# Find cmdlets with specific noun
Get-Command -Noun Service

# Search with wildcards
Get-Command *Service*
Get-Command Get-Net*

# Filter by command type
Get-Command -CommandType Cmdlet
Get-Command -CommandType Function
Get-Command -CommandType Alias
```

**Practical Example:**
```powershell
# Find all network-related cmdlets
PS C:\> Get-Command *Net*

# Output shows cmdlets like:
# Get-NetAdapter
# Get-NetIPAddress
# Get-NetTCPConnection
# Test-NetConnection
```

#### **Get-Help** - Access Documentation
```powershell
# Basic help
Get-Help Get-Process

# Detailed help with examples
Get-Help Get-Process -Detailed

# View examples only
Get-Help Get-Process -Examples

# Open full help in browser
Get-Help Get-Process -Online

# Get help about PowerShell concepts
Get-Help about_Pipelines
Get-Help about_Variables
```

**Best Practice:**
Always check `Get-Help` before using unfamiliar cmdlets. It shows:
- Syntax and parameters
- Practical examples
- Parameter descriptions
- Related cmdlets

#### **Get-Date** - Work with Dates and Times
```powershell
# Get current date and time
Get-Date

# Format output
Get-Date -Format "yyyy-MM-dd"
Get-Date -Format "HH:mm:ss"

# Get specific date component
(Get-Date).Year
(Get-Date).DayOfWeek

# Date arithmetic (useful for log filtering)
$yesterday = (Get-Date).AddDays(-1)
$lastWeek = (Get-Date).AddDays(-7)
```

#### **Clear-Host** - Clear Console Screen
```powershell
# Clear the screen
Clear-Host

# Keyboard shortcut: Ctrl+L
```

### PowerShell Modules

Modules extend PowerShell functionality by adding new cmdlets.

#### **Get-Module** - List and Manage Modules
```powershell
# Show currently loaded modules
Get-Module

# Show all available modules (installed but not loaded)
Get-Module -ListAvailable

# Find specific module
Get-Module -Name PowerShellGet

# Load a module
Import-Module ModuleName
```

#### **Finding and Installing Modules**

**PowerShell Gallery** (https://www.powershellgallery.com/)
- Official Microsoft repository for PowerShell modules
- Thousands of community and official modules available
- Install modules with `Install-Module`

```powershell
# Search for modules
Find-Module -Name *Azure*

# Install module (requires admin privileges)
Install-Module -Name Az -Scope CurrentUser

# Update installed module
Update-Module -Name Az
```

**Common Useful Modules:**
- **Az** - Azure resource management
- **ActiveDirectory** - AD management (Windows Server feature)
- **Microsoft.Graph** - Microsoft 365 management
- **Pester** - PowerShell testing framework
- **ImportExcel** - Work with Excel files without Excel installed

---

## Task 4: Navigating the File System and Working with Files

### Get-ChildItem - The PowerShell "dir" Command

`Get-ChildItem` (alias: `ls`, `dir`) lists files and directories.

#### **Basic Usage**
```powershell
# List current directory
Get-ChildItem

# List specific path
Get-ChildItem C:\Windows

# Use alias for brevity
ls C:\Users
dir C:\Program Files
```

#### **Common Parameters**

**-Path** - Specify location
```powershell
Get-ChildItem -Path C:\Windows\System32
```

**-Recurse** - Include subdirectories
```powershell
# List all files in directory tree
Get-ChildItem -Path C:\Logs -Recurse

# Find all .txt files recursively
Get-ChildItem -Path C:\ -Recurse -Filter *.txt
```

**-Filter** - Filter by name pattern
```powershell
# Find all .log files
Get-ChildItem -Filter *.log

# Find all files starting with "report"
Get-ChildItem -Filter report*
```

**-Name** - Show only names (not full objects)
```powershell
Get-ChildItem -Name
# Output: simple list of names
```

**-Force** - Show hidden/system files
```powershell
Get-ChildItem -Force
# Reveals hidden files, system files, and hidden directories
```

**-Directory** - Show only directories
```powershell
Get-ChildItem -Directory
```

**-File** - Show only files
```powershell
Get-ChildItem -File
```

#### **Practical Examples**

**Find large files:**
```powershell
# Files larger than 100MB
Get-ChildItem -Recurse | Where-Object {$_.Length -gt 100MB}
```

**Find recently modified files:**
```powershell
# Files modified in last 7 days
$date = (Get-Date).AddDays(-7)
Get-ChildItem -Recurse | Where-Object {$_.LastWriteTime -gt $date}
```

**Search for specific file type:**
```powershell
# All PowerShell scripts in system
Get-ChildItem C:\ -Recurse -Filter *.ps1 -ErrorAction SilentlyContinue
```

### Pipelines and Object Manipulation

PowerShell's pipeline passes **objects** between cmdlets, enabling powerful data manipulation.

#### **Sort-Object** - Sort Results
```powershell
# Sort by name (default)
Get-ChildItem | Sort-Object

# Sort by size (largest first)
Get-ChildItem | Sort-Object Length -Descending

# Sort by last modified date
Get-ChildItem | Sort-Object LastWriteTime
```

#### **Select-Object** - Choose Properties
```powershell
# Show only Name and Length
Get-ChildItem | Select-Object Name, Length

# Get first 10 items
Get-ChildItem | Select-Object -First 10

# Get last 5 items
Get-ChildItem | Select-Object -Last 5

# Select specific properties with custom names
Get-ChildItem | Select-Object Name, 
    @{Name='SizeMB';Expression={$_.Length / 1MB}}
```

#### **Measure-Object** - Calculate Statistics
```powershell
# Count files
Get-ChildItem -File | Measure-Object

# Calculate total size
Get-ChildItem -File | Measure-Object -Property Length -Sum

# Get statistics (Count, Sum, Average, Min, Max)
Get-ChildItem | Measure-Object -Property Length -Sum -Average -Maximum
```

#### **Where-Object** - Filter Results
```powershell
# Files larger than 1MB
Get-ChildItem | Where-Object {$_.Length -gt 1MB}

# Files modified today
Get-ChildItem | Where-Object {$_.LastWriteTime -gt (Get-Date).Date}

# Only .txt files
Get-ChildItem | Where-Object {$_.Extension -eq '.txt'}
```

### Real-World Pipeline Examples

**Find 10 largest files in directory:**
```powershell
Get-ChildItem -Recurse -File | 
    Sort-Object Length -Descending | 
    Select-Object -First 10 Name, 
        @{Name='SizeMB';Expression={[math]::Round($_.Length / 1MB, 2)}}
```

**Calculate total directory size:**
```powershell
$totalSize = Get-ChildItem -Recurse -File | 
    Measure-Object -Property Length -Sum
$totalSizeMB = [math]::Round($totalSize.Sum / 1MB, 2)
Write-Host "Total size: $totalSizeMB MB"
```

**Count files by extension:**
```powershell
Get-ChildItem -Recurse -File | 
    Group-Object Extension | 
    Select-Object Name, Count | 
    Sort-Object Count -Descending
```

---

## Task 5: System and Network Information

### Get-ComputerInfo - Comprehensive System Details

`Get-ComputerInfo` retrieves extensive system information in a single command.

```powershell
# Get all system information
Get-ComputerInfo

# Get specific properties
Get-ComputerInfo | Select-Object CsName, WindowsVersion, OsArchitecture

# Find OS build number
(Get-ComputerInfo).WindowsBuildLabEx
```

**Key Properties:**
- **CsName** - Computer name
- **WindowsVersion** - Windows version (e.g., 2009 for Windows 10 20H2)
- **OsArchitecture** - 64-bit or 32-bit
- **CsManufacturer** - Computer manufacturer
- **CsModel** - Computer model
- **CsProcessors** - CPU information
- **CsTotalPhysicalMemory** - RAM size
- **BiosFirmwareType** - UEFI or Legacy BIOS

### Get-LocalUser - User Account Management

```powershell
# List all local users
Get-LocalUser

# Get specific user
Get-LocalUser -Name Administrator

# Check if account is enabled
Get-LocalUser | Where-Object {$_.Enabled -eq $true}

# Find accounts with passwords that never expire
Get-LocalUser | Where-Object {$_.PasswordNeverExpires -eq $true}
```

**Key Properties:**
- **Name** - Username
- **Enabled** - Account active status
- **PasswordRequired** - Whether password is required
- **PasswordNeverExpires** - Password expiration policy
- **LastLogon** - Last login time
- **Description** - Account description

**Security Audit Example:**
```powershell
# Find enabled accounts with no password required
Get-LocalUser | Where-Object {
    $_.Enabled -and -not $_.PasswordRequired
}
```

### Network Information Cmdlets

#### **Get-NetAdapter** - Network Interface Cards
```powershell
# List all network adapters
Get-NetAdapter

# Show only active adapters
Get-NetAdapter | Where-Object {$_.Status -eq 'Up'}

# Get specific adapter
Get-NetAdapter -Name Ethernet

# Check link speed
Get-NetAdapter | Select-Object Name, Status, LinkSpeed
```

#### **Get-NetIPAddress** - IP Configuration
```powershell
# Show all IP addresses
Get-NetIPAddress

# IPv4 addresses only
Get-NetIPAddress -AddressFamily IPv4

# Get IP for specific adapter
Get-NetAdapter -Name Ethernet | Get-NetIPAddress

# Find gateway
Get-NetRoute | Where-Object {$_.DestinationPrefix -eq '0.0.0.0/0'}
```

**Troubleshooting Connectivity:**
```powershell
# Quick network check
Get-NetAdapter | Select-Object Name, Status
Get-NetIPAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, IPAddress
Test-NetConnection -ComputerName google.com
```

#### **Get-NetNeighbor** - ARP Cache
```powershell
# Show ARP cache (IP to MAC mappings)
Get-NetNeighbor

# Show reachable neighbors only
Get-NetNeighbor | Where-Object {$_.State -eq 'Reachable'}

# Find specific IP
Get-NetNeighbor -IPAddress 192.168.1.1
```

**Network Discovery Example:**
```powershell
# Discover devices on local network
Get-NetNeighbor -AddressFamily IPv4 | 
    Where-Object {$_.State -eq 'Reachable'} |
    Select-Object IPAddress, LinkLayerAddress, State
```

---

## Task 6: Managing Processes and Services

### Get-Service - Windows Services Management

```powershell
# List all services
Get-Service

# Find running services
Get-Service | Where-Object {$_.Status -eq 'Running'}

# Find stopped services
Get-Service | Where-Object {$_.Status -eq 'Stopped'}

# Search for specific service
Get-Service *Windows*

# Check specific service status
Get-Service -Name Spooler
```

**Service Management:**
```powershell
# Start service (requires admin)
Start-Service -Name Spooler

# Stop service (requires admin)
Stop-Service -Name Spooler

# Restart service (requires admin)
Restart-Service -Name Spooler

# Change startup type (requires admin)
Set-Service -Name Spooler -StartupType Automatic
```

**Practical Example - Audit Critical Services:**
```powershell
# Check if critical services are running
$criticalServices = @('Spooler', 'W32Time', 'Winmgmt')
foreach ($service in $criticalServices) {
    $status = Get-Service -Name $service
    Write-Host "$($status.Name): $($status.Status)"
}
```

### Get-Process - Running Processes

```powershell
# List all processes
Get-Process

# Find specific process
Get-Process -Name chrome

# Sort by memory usage (highest first)
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10

# Sort by CPU usage
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# Get process by ID
Get-Process -Id 1234
```

**Memory and CPU Analysis:**
```powershell
# Find processes using > 500MB RAM
Get-Process | Where-Object {$_.WorkingSet -gt 500MB} | 
    Select-Object Name, 
        @{Name='MemoryMB';Expression={[math]::Round($_.WorkingSet / 1MB, 2)}}

# Process count by name
Get-Process | Group-Object Name | 
    Sort-Object Count -Descending | 
    Select-Object -First 10
```

### Get-NetTCPConnection - Network Connections

```powershell
# List all TCP connections
Get-NetTCPConnection

# Show established connections
Get-NetTCPConnection -State Established

# Find connections on specific port
Get-NetTCPConnection -LocalPort 80

# Find what's using a port
Get-NetTCPConnection -LocalPort 443 | 
    Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, 
        @{Name='Process';Expression={(Get-Process -Id $_.OwningProcess).ProcessName}}
```

**Security Monitoring Example:**
```powershell
# Find unexpected listening ports
Get-NetTCPConnection -State Listen | 
    Where-Object {$_.LocalPort -notin @(80, 443, 3389, 22)} |
    Select-Object LocalAddress, LocalPort, 
        @{Name='Process';Expression={(Get-Process -Id $_.OwningProcess).Name}}
```

### Get-EventLog - Windows Event Logs

**Note:** `Get-EventLog` works on Windows PowerShell 5.1. For PowerShell 7+, use `Get-WinEvent`.

```powershell
# List available logs
Get-EventLog -List

# Get recent System log entries
Get-EventLog -LogName System -Newest 50

# Get recent Application errors
Get-EventLog -LogName Application -EntryType Error -Newest 20

# Filter by time
$yesterday = (Get-Date).AddDays(-1)
Get-EventLog -LogName System -After $yesterday

# Find specific event ID
Get-EventLog -LogName System -InstanceId 1074 -Newest 10
```

**Troubleshooting Example:**
```powershell
# Find errors in last 24 hours
$startTime = (Get-Date).AddHours(-24)
Get-EventLog -LogName System -EntryType Error -After $startTime | 
    Select-Object TimeGenerated, Source, EventID, Message |
    Format-Table -AutoSize
```

**Security Audit Example:**
```powershell
# Find failed login attempts (Security log - requires admin)
Get-EventLog -LogName Security | 
    Where-Object {$_.EventID -eq 4625} | 
    Select-Object TimeGenerated, Message -First 10
```

---

## Task 7: Scripting

### PowerShell Scripts (.ps1 Files)

PowerShell scripts are text files with `.ps1` extension containing PowerShell commands.

#### **Creating a Script**
```powershell
# Option 1: Use Notepad
notepad C:\Scripts\myscript.ps1

# Option 2: Use PowerShell ISE (Integrated Scripting Environment)
# Start Menu > Windows PowerShell ISE

# Option 3: Use VS Code with PowerShell extension
code C:\Scripts\myscript.ps1
```

#### **Simple Script Example**
```powershell
# File: SystemCheck.ps1
# Purpose: Quick system health check

Write-Host "=== System Health Check ===" -ForegroundColor Green

# Disk space
$disk = Get-PSDrive C
Write-Host "C: Drive Free Space: $([math]::Round($disk.Free / 1GB, 2)) GB"

# Memory usage
$os = Get-CimInstance Win32_OperatingSystem
$memUsedPercent = (($os.TotalVisibleMemorySize - $os.FreePhysicalMemory) / $os.TotalVisibleMemorySize) * 100
Write-Host "Memory Usage: $([math]::Round($memUsedPercent, 2))%"

# CPU high users
Write-Host "`nTop 5 CPU Processes:"
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, CPU
```

#### **Running Scripts**

**Execution Policy Check:**
```powershell
# Check current policy
Get-ExecutionPolicy

# Set policy (run as admin)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Execution Policies:**
- **Restricted** - No scripts allowed (default)
- **AllSigned** - Only signed scripts
- **RemoteSigned** - Local scripts OK, downloaded must be signed
- **Unrestricted** - All scripts allowed (not recommended)

**Run Script:**
```powershell
# Method 1: Full path
C:\Scripts\SystemCheck.ps1

# Method 2: Relative path (must include .\ prefix)
.\SystemCheck.ps1

# Method 3: Call operator
& "C:\Scripts\System Check.ps1"  # For paths with spaces
```

### Invoke-Command - Remote Script Execution

`Invoke-Command` executes commands/scripts on remote computers.

#### **Basic Remote Execution**
```powershell
# Run command on remote computer
Invoke-Command -ComputerName Server01 -ScriptBlock {
    Get-Service -Name Spooler
}

# Run on multiple computers
Invoke-Command -ComputerName Server01, Server02, Server03 -ScriptBlock {
    Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
}
```

#### **Prerequisites for Remoting:**
1. **Enable PSRemoting** (run as admin on target):
```powershell
Enable-PSRemoting -Force
```

2. **Configure TrustedHosts** (if not in domain):
```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "Server01,Server02"
```

3. **Firewall** - WinRM ports must be open (5985 HTTP, 5986 HTTPS)

#### **Remote Script with Credentials**
```powershell
$cred = Get-Credential
Invoke-Command -ComputerName Server01 -Credential $cred -ScriptBlock {
    Get-EventLog -LogName System -Newest 10
}
```

#### **Run Local Script on Remote Computer**
```powershell
Invoke-Command -ComputerName Server01 -FilePath C:\Scripts\DiskCheck.ps1
```

### Automation Use Cases

**1. Scheduled Health Checks:**
```powershell
# Monitor disk space across servers
$servers = @('Server01', 'Server02', 'Server03')
Invoke-Command -ComputerName $servers -ScriptBlock {
    $disk = Get-PSDrive C
    [PSCustomObject]@{
        Computer = $env:COMPUTERNAME
        FreeSpaceGB = [math]::Round($disk.Free / 1GB, 2)
    }
} | Where-Object {$_.FreeSpaceGB -lt 10} | 
    Format-Table -AutoSize
```

**2. User Account Auditing:**
```powershell
# Find accounts with passwords never expiring
Get-LocalUser | 
    Where-Object {$_.PasswordNeverExpires -and $_.Enabled} |
    Select-Object Name, LastLogon, PasswordLastSet |
    Export-Csv C:\Reports\PasswordAudit.csv -NoTypeInformation
```

**3. Service Monitoring:**
```powershell
# Alert if critical service stops
$criticalServices = @('W32Time', 'Winmgmt', 'Spooler')
$stoppedServices = Get-Service $criticalServices | 
    Where-Object {$_.Status -ne 'Running'}

if ($stoppedServices) {
    Send-MailMessage -To admin@company.com `
        -Subject "Critical Service Alert" `
        -Body "Stopped services: $($stoppedServices.Name -join ', ')"
}
```

**4. Log Collection:**
```powershell
# Collect errors from multiple servers
$servers = @('Server01', 'Server02', 'Server03')
$yesterday = (Get-Date).AddDays(-1)

Invoke-Command -ComputerName $servers -ScriptBlock {
    Get-EventLog -LogName System -EntryType Error -After $using:yesterday
} | Export-Csv C:\Reports\SystemErrors.csv -NoTypeInformation
```

### Best Practices for Scripting

**1. Use Comments:**
```powershell
# Single line comment
<#
Multi-line
comment
block
#>
```

**2. Error Handling:**
```powershell
try {
    Get-Service -Name NonExistentService -ErrorAction Stop
}
catch {
    Write-Host "Error: $($_.Exception.Message)" -ForegroundColor Red
}
```

**3. Parameters for Reusability:**
```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$ComputerName,
    
    [int]$Days = 7
)

# Use parameters in script
Get-EventLog -ComputerName $ComputerName -LogName System -After (Get-Date).AddDays(-$Days)
```

**4. Output Formatting:**
```powershell
# Format table for readability
Get-Process | Select-Object -First 10 | Format-Table -AutoSize

# Export to CSV for Excel
Get-Service | Export-Csv C:\Reports\Services.csv -NoTypeInformation

# Export to HTML report
Get-Process | ConvertTo-Html | Out-File C:\Reports\Processes.html
```

---

## Summary: Key PowerShell Concepts

### **1. Object-Oriented Pipeline**
Unlike cmd.exe's text, PowerShell passes structured objects between commands, enabling powerful data manipulation without text parsing.

### **2. Verb-Noun Consistency**
All cmdlets follow predictable naming (Get-Service, Set-Location), making discovery intuitive.

### **3. Built-in Help**
`Get-Help` and `Get-Command` provide comprehensive documentation and cmdlet discovery.

### **4. Remote Management**
`Invoke-Command` enables managing multiple systems from single console—essential for enterprise environments.

### **5. Automation Potential**
Scripts (.ps1) automate repetitive tasks, scheduled jobs, and complex workflows.

---

## Career-Relevant Takeaways

**For Junior Sysadmin Roles:**
- PowerShell is **mandatory** for Windows administration
- Remote management capabilities = competitive advantage
- Scripting ability = higher salary potential

**For Security Roles:**
- Event log analysis with PowerShell
- Network connection monitoring
- Security audit automation

**For DevOps/Automation:**
- Infrastructure as Code (IaC) with PowerShell
- CI/CD pipeline integration
- Configuration management automation

**Certifications Where PowerShell Appears:**
- CompTIA Server+
- Microsoft Azure Administrator (AZ-104)
- Microsoft 365 Certified Administrator
- MCSA (legacy but still relevant)

---

**Master PowerShell = Unlock Windows automation. Essential skill for any IT operations role.**
