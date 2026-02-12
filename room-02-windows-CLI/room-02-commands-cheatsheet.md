# PowerShell Commands Cheatsheet

**TryHackMe - Windows Command Line: Room 2**  
**Quick Reference for System Administrators**

---

## Essential PowerShell Cmdlets

### Discovery & Help
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-Command` | Find available cmdlets | `Get-Command *Service*` |
| `Get-Help` | View documentation | `Get-Help Get-Process -Examples` |
| `Get-Module` | List modules | `Get-Module -ListAvailable` |
| `Update-Help` | Download latest help | `Update-Help` (requires admin) |

### Navigation & Files
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-ChildItem` | List files/folders | `Get-ChildItem -Recurse -Filter *.log` |
| `Set-Location` | Change directory | `Set-Location C:\Windows` |
| `Get-Location` | Show current path | `Get-Location` |
| `New-Item` | Create file/folder | `New-Item -ItemType Directory -Path C:\Temp` |
| `Remove-Item` | Delete file/folder | `Remove-Item C:\Temp\file.txt` |
| `Copy-Item` | Copy file/folder | `Copy-Item source.txt destination.txt` |
| `Move-Item` | Move/rename | `Move-Item old.txt new.txt` |
| `Get-Content` | Read file | `Get-Content C:\Logs\app.log` |
| `Out-File` | Write to file | `Get-Process \| Out-File processes.txt` |

### System Information
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-ComputerInfo` | Comprehensive system info | `Get-ComputerInfo \| Select-Object CsName, WindowsVersion` |
| `Get-LocalUser` | List local users | `Get-LocalUser` |
| `Get-LocalGroup` | List local groups | `Get-LocalGroup` |
| `Get-HotFix` | Installed updates | `Get-HotFix \| Sort-Object InstalledOn -Descending` |
| `Get-Date` | Current date/time | `Get-Date -Format "yyyy-MM-dd HH:mm"` |
| `systeminfo` | Legacy system info | `systeminfo` (cmd.exe command still works) |

### Network Operations
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-NetAdapter` | Network interfaces | `Get-NetAdapter \| Where-Object {$_.Status -eq 'Up'}` |
| `Get-NetIPAddress` | IP configuration | `Get-NetIPAddress -AddressFamily IPv4` |
| `Get-NetRoute` | Routing table | `Get-NetRoute` |
| `Get-NetNeighbor` | ARP cache | `Get-NetNeighbor -AddressFamily IPv4` |
| `Get-NetTCPConnection` | Active connections | `Get-NetTCPConnection -State Established` |
| `Test-NetConnection` | Connectivity test | `Test-NetConnection -ComputerName google.com -Port 443` |
| `Resolve-DnsName` | DNS lookup | `Resolve-DnsName google.com` |
| `Test-Connection` | Ping | `Test-Connection -ComputerName 192.168.1.1 -Count 4` |

### Processes & Services
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-Process` | Running processes | `Get-Process \| Sort-Object CPU -Descending` |
| `Stop-Process` | Kill process | `Stop-Process -Name notepad -Force` |
| `Start-Process` | Launch process | `Start-Process notepad.exe` |
| `Get-Service` | List services | `Get-Service \| Where-Object {$_.Status -eq 'Running'}` |
| `Start-Service` | Start service | `Start-Service -Name Spooler` |
| `Stop-Service` | Stop service | `Stop-Service -Name Spooler` |
| `Restart-Service` | Restart service | `Restart-Service -Name Spooler` |
| `Set-Service` | Configure service | `Set-Service -Name Spooler -StartupType Automatic` |

### Event Logs
| Command | Purpose | Example |
|---------|---------|---------|
| `Get-EventLog` | Read event logs (Win PS 5.1) | `Get-EventLog -LogName System -Newest 50` |
| `Get-WinEvent` | Read event logs (PS 7+) | `Get-WinEvent -LogName System -MaxEvents 50` |
| `Clear-EventLog` | Clear log (requires admin) | `Clear-EventLog -LogName Application` |

### Remote Management
| Command | Purpose | Example |
|---------|---------|---------|
| `Invoke-Command` | Execute remotely | `Invoke-Command -ComputerName Server01 -ScriptBlock {Get-Service}` |
| `Enter-PSSession` | Interactive remote session | `Enter-PSSession -ComputerName Server01` |
| `Exit-PSSession` | Exit remote session | `Exit-PSSession` |
| `New-PSSession` | Create persistent session | `$s = New-PSSession -ComputerName Server01` |
| `Enable-PSRemoting` | Enable remoting | `Enable-PSRemoting -Force` (requires admin) |

---

## Common Scenarios: "How Do I...?"

### File & Directory Operations
| Scenario | Command |
|----------|---------|
| List all files in directory? | `Get-ChildItem` or `ls` or `dir` |
| Include subdirectories? | `Get-ChildItem -Recurse` |
| Find all .txt files? | `Get-ChildItem -Recurse -Filter *.txt` |
| Show hidden files? | `Get-ChildItem -Force` |
| Find files > 100MB? | `Get-ChildItem -Recurse \| Where-Object {$_.Length -gt 100MB}` |
| Find 10 largest files? | `Get-ChildItem -Recurse \| Sort-Object Length -Descending \| Select-Object -First 10` |
| Calculate folder size? | `(Get-ChildItem -Recurse \| Measure-Object -Property Length -Sum).Sum / 1GB` |
| Count files by type? | `Get-ChildItem -Recurse \| Group-Object Extension \| Sort-Object Count -Descending` |

### System Administration
| Scenario | Command |
|----------|---------|
| What's my computer name? | `$env:COMPUTERNAME` or `hostname` |
| What's my OS version? | `(Get-ComputerInfo).WindowsVersion` |
| How much RAM installed? | `(Get-ComputerInfo).CsTotalPhysicalMemory / 1GB` |
| Check disk space? | `Get-PSDrive C` |
| Find installed updates? | `Get-HotFix \| Sort-Object InstalledOn -Descending` |
| List local users? | `Get-LocalUser` |
| Check if user enabled? | `Get-LocalUser -Name Username \| Select-Object Enabled` |

### Network Troubleshooting
| Scenario | Command |
|----------|---------|
| What's my IP address? | `Get-NetIPAddress -AddressFamily IPv4` |
| What's my gateway? | `Get-NetRoute \| Where-Object {$_.DestinationPrefix -eq '0.0.0.0/0'}` |
| Are adapters up? | `Get-NetAdapter \| Select-Object Name, Status` |
| Test connectivity? | `Test-NetConnection -ComputerName google.com` |
| Check open ports? | `Get-NetTCPConnection -State Listen` |
| Find what's using port 80? | `Get-NetTCPConnection -LocalPort 80` |
| Check DNS resolution? | `Resolve-DnsName google.com` |

### Process Management
| Scenario | Command |
|----------|---------|
| What's using most CPU? | `Get-Process \| Sort-Object CPU -Descending \| Select-Object -First 10` |
| What's using most memory? | `Get-Process \| Sort-Object WorkingSet -Descending \| Select-Object -First 10` |
| Kill frozen process? | `Stop-Process -Name processname -Force` |
| Kill by PID? | `Stop-Process -Id 1234` |
| Find Chrome processes? | `Get-Process -Name chrome` |

### Service Management
| Scenario | Command |
|----------|---------|
| Is service running? | `Get-Service -Name ServiceName` |
| Start service? | `Start-Service -Name ServiceName` |
| Stop service? | `Stop-Service -Name ServiceName` |
| Restart service? | `Restart-Service -Name ServiceName` |
| Set to auto-start? | `Set-Service -Name ServiceName -StartupType Automatic` |
| Find stopped services? | `Get-Service \| Where-Object {$_.Status -eq 'Stopped'}` |

### Event Log Analysis
| Scenario | Command |
|----------|---------|
| Recent System errors? | `Get-EventLog -LogName System -EntryType Error -Newest 20` |
| Errors in last 24 hours? | `Get-EventLog -LogName System -After (Get-Date).AddHours(-24) -EntryType Error` |
| Find specific event ID? | `Get-EventLog -LogName System -InstanceId 1074` |
| Export to CSV? | `Get-EventLog -LogName System -Newest 100 \| Export-Csv errors.csv` |

### Remote Management
| Scenario | Command |
|----------|---------|
| Run command remotely? | `Invoke-Command -ComputerName Server01 -ScriptBlock {Get-Service}` |
| Check disk on remote? | `Invoke-Command -ComputerName Server01 -ScriptBlock {Get-PSDrive C}` |
| Interactive session? | `Enter-PSSession -ComputerName Server01` |
| Run script remotely? | `Invoke-Command -ComputerName Server01 -FilePath C:\Scripts\script.ps1` |

---

## Pipeline Essentials

### Core Pipeline Cmdlets
| Cmdlet | Purpose | Example |
|--------|---------|---------|
| `Where-Object` | Filter results | `Get-Process \| Where-Object {$_.CPU -gt 100}` |
| `Select-Object` | Choose properties | `Get-Process \| Select-Object Name, CPU` |
| `Sort-Object` | Sort results | `Get-Process \| Sort-Object CPU -Descending` |
| `Measure-Object` | Calculate stats | `Get-ChildItem \| Measure-Object -Property Length -Sum` |
| `Group-Object` | Group by property | `Get-Process \| Group-Object Company` |
| `Format-Table` | Table view | `Get-Process \| Format-Table -AutoSize` |
| `Format-List` | List view | `Get-Service \| Format-List` |
| `Out-GridView` | Interactive GUI | `Get-Process \| Out-GridView` |

### Pipeline Examples
```powershell
# Find top 5 memory consumers
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 5

# Count running services
Get-Service | Where-Object {$_.Status -eq 'Running'} | Measure-Object

# Find large log files and calculate total
Get-ChildItem C:\Logs -Recurse -Filter *.log | 
    Where-Object {$_.Length -gt 10MB} |
    Measure-Object -Property Length -Sum

# List processes grouped by company
Get-Process | 
    Group-Object Company | 
    Sort-Object Count -Descending |
    Select-Object Name, Count
```

---

## PowerShell Operators & Variables

### Comparison Operators
| Operator | Meaning | Example |
|----------|---------|---------|
| `-eq` | Equal | `$a -eq $b` |
| `-ne` | Not equal | `$a -ne $b` |
| `-gt` | Greater than | `$_.Length -gt 1MB` |
| `-ge` | Greater or equal | `$_.CPU -ge 100` |
| `-lt` | Less than | `$_.Count -lt 10` |
| `-le` | Less or equal | `$_.Size -le 500KB` |
| `-like` | Wildcard match | `$_.Name -like "*.txt"` |
| `-match` | Regex match | `$_.Path -match "C:\\\\Logs"` |
| `-contains` | Array contains | `$array -contains "value"` |
| `-in` | Value in array | `"value" -in $array` |

### Variables
```powershell
# Create variable
$name = "Server01"
$count = 5

# Environment variables
$env:COMPUTERNAME
$env:USERNAME
$env:PATH

# Automatic variables
$PSVersionTable    # PowerShell version
$PWD              # Current directory
$HOME             # User home directory
$_                # Current pipeline object
$?                # Last command success (true/false)
```

---

## Quick Tips & Tricks

### Aliases
PowerShell has built-in aliases for common commands:
```powershell
ls    → Get-ChildItem
dir   → Get-ChildItem
cd    → Set-Location
pwd   → Get-Location
cat   → Get-Content
cp    → Copy-Item
mv    → Move-Item
rm    → Remove-Item
man   → Get-Help
cls   → Clear-Host
```

View all aliases: `Get-Alias`

### Tab Completion
- Press **Tab** to auto-complete cmdlet names, parameters, and paths
- Press **Ctrl+Space** to show all options

### Command History
```powershell
# View history
Get-History

# Run previous command
Invoke-History 1

# Search history
Get-History | Where-Object {$_.CommandLine -like "*Service*"}

# Keyboard shortcuts:
# Up/Down Arrow - Navigate history
# F7 - Interactive history menu
# F8 - Search history (type partial, press F8)
```

### Execution Policy (for Scripts)
```powershell
# Check current policy
Get-ExecutionPolicy

# Set policy (run as admin)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser

# Bypass for single script
PowerShell.exe -ExecutionPolicy Bypass -File script.ps1
```

### Output Redirection
```powershell
# Save to file (overwrite)
Get-Process > processes.txt

# Append to file
Get-Service >> services.txt

# Specific cmdlets
Get-Process | Out-File processes.txt
Get-Process | Export-Csv processes.csv -NoTypeInformation
Get-Process | ConvertTo-Html | Out-File processes.html
```

### Filtering vs Selecting
```powershell
# Where-Object filters ROWS (reduces object count)
Get-Process | Where-Object {$_.CPU -gt 100}

# Select-Object filters COLUMNS (reduces properties)
Get-Process | Select-Object Name, CPU
```

### Measuring Performance
```powershell
# Time a command
Measure-Command { Get-ChildItem C:\ -Recurse }

# Shows TotalMilliseconds, TotalSeconds, etc.
```

---

## Common Scenarios for Sysadmins

### Daily Health Check Script
```powershell
Write-Host "=== System Health Check ===" -ForegroundColor Green

# Disk space
$disk = Get-PSDrive C
$freeGB = [math]::Round($disk.Free / 1GB, 2)
Write-Host "C: Free Space: $freeGB GB"

# Memory
$os = Get-CimInstance Win32_OperatingSystem
$memPercent = (($os.TotalVisibleMemorySize - $os.FreePhysicalMemory) / $os.TotalVisibleMemorySize) * 100
Write-Host "Memory Usage: $([math]::Round($memPercent, 2))%"

# CPU hogs
Write-Host "`nTop 5 CPU Processes:"
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5 Name, CPU
```

### Find Failed Services
```powershell
$criticalServices = @('Spooler', 'W32Time', 'Winmgmt', 'WinRM')
$stoppedServices = Get-Service $criticalServices | Where-Object {$_.Status -ne 'Running'}

if ($stoppedServices) {
    Write-Host "ALERT: Critical services stopped!" -ForegroundColor Red
    $stoppedServices | Format-Table Name, Status
}
```

### Network Port Scan
```powershell
# Check common ports on target
$ports = @(80, 443, 3389, 22, 445)
foreach ($port in $ports) {
    $result = Test-NetConnection -ComputerName 192.168.1.1 -Port $port -WarningAction SilentlyContinue
    Write-Host "Port $port : $($result.TcpTestSucceeded)"
}
```

### Clean Old Files
```powershell
# Delete files older than 30 days
$path = "C:\Logs"
$days = 30
$cutoff = (Get-Date).AddDays(-$days)

Get-ChildItem -Path $path -Recurse -File | 
    Where-Object {$_.LastWriteTime -lt $cutoff} |
    Remove-Item -Force -WhatIf  # Remove -WhatIf to actually delete
```

---

## Command Frequency Guide

### Use Daily
- `Get-ChildItem` (ls/dir)
- `Get-Service`
- `Get-Process`
- `Set-Location` (cd)
- `Get-Help`
- `Get-NetAdapter`

### Use Weekly
- `Get-ComputerInfo`
- `Get-EventLog` / `Get-WinEvent`
- `Get-HotFix`
- `Get-NetTCPConnection`
- `Invoke-Command`

### Use for Troubleshooting
- `Test-NetConnection`
- `Resolve-DnsName`
- `Get-NetNeighbor`
- `Stop-Process`
- `Restart-Service`
- `Measure-Object`

### Use for Automation
- `Invoke-Command`
- `Export-Csv`
- `ConvertTo-Html`
- `Send-MailMessage`
- Script blocks with `foreach`

---

## PowerShell vs cmd.exe Quick Comparison

| Task | cmd.exe | PowerShell |
|------|---------|------------|
| List files | `dir` | `Get-ChildItem` or `dir` |
| Change directory | `cd` | `Set-Location` or `cd` |
| Copy file | `copy` | `Copy-Item` |
| Delete file | `del` | `Remove-Item` |
| Find text in file | `findstr` | `Select-String` |
| View IP config | `ipconfig` | `Get-NetIPAddress` |
| Check services | `sc query` | `Get-Service` |
| Kill process | `taskkill` | `Stop-Process` |
| System info | `systeminfo` | `Get-ComputerInfo` |

**When to use cmd.exe:** Legacy scripts, basic file operations, compatibility with old tools  
**When to use PowerShell:** Automation, remote management, complex data manipulation, modern Windows administration

---

## Resources

**Official Documentation:**
- Microsoft PowerShell Docs: https://docs.microsoft.com/powershell
- PowerShell Gallery: https://www.powershellgallery.com

**Learning:**
- `Get-Help about_*` - Built-in concept help
- `Get-Command -Module ModuleName` - Discover module cmdlets
- PowerShell in a Month of Lunches (book)

**Practice:**
- TryHackMe PowerShell rooms
- OverTheWire PowerShell challenges
- Build your own automation scripts

---

**Master these commands = Efficient Windows system administration. Print this and keep it at your desk!**
