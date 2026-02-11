# TryHackMe Windows Command Line - Room 1: Quick Reference Cheatsheet

Fast reference for essential Windows command-line commands from Room 1.

---

## Opening Command Prompt

### Methods

```
Win+R → cmd → Enter
Windows search → "Command Prompt"
Right-click Start menu → Windows Terminal (or Command Prompt)
Win+R → powershell → Enter (modern alternative)
```

### Help for Any Command

```
command /?
systeminfo /?
ipconfig /?
tasklist /?
```

---

## System Information Commands

### Quick System Info

```
systeminfo                    Show all system details
systeminfo | find "OS"        Just show OS info
systeminfo | find "Memory"    Just show RAM info
systeminfo /s computername   Query remote system
```

### What systeminfo Shows

```
OS Name                   What Windows version
OS Version                Build number (for updates)
System Boot Time          When system started (uptime)
Processor                 CPU model and speed
Total Physical Memory     Total RAM installed
```

### Alternative System Queries

```
wmic os get caption,version,buildnumber    WMI query
ver                                        Just OS version
tasklist /svc                             Processes and services
Get-ComputerInfo (PowerShell)             Detailed system info
```

### Remote System Queries

```
systeminfo /s othermachine                Query remote
systeminfo /s othermachine /u domain\user /p password   With login
```

---

## Network Commands

### IP Configuration

```
ipconfig                    Quick IP info
ipconfig /all               Detailed network info
ipconfig /renew            Request new DHCP IP
ipconfig /release           Release current DHCP IP
ipconfig /release /renew    Force IP renewal
```

### What ipconfig Shows

```
IPv4 Address              Your IP address (like 192.168.1.100)
Subnet Mask              Network range (usually 255.255.255.0)
Default Gateway          Router IP (how to reach internet)
DNS Servers              Domain name servers (internet addresses)
DHCP Server             Where IP came from
Physical Address         MAC address
```

### Routing

```
route print               Show all routes
route print | find "0.0.0.0"   Just default route
route add destination mask gateway   Add new route
route delete destination            Remove route
```

### DNS Lookups

```
nslookup google.com               What IP is google.com?
nslookup google.com 8.8.8.8       Query specific DNS server
ipconfig /flushdns                Clear DNS cache
```

### MAC Addresses

```
arp -a                    Show all MAC/IP mappings
getmac                    Just your MAC address
getmac /s computername   Remote MAC address
```

### Network Testing

```
ping google.com           Can I reach this host?
tracert google.com        Where does path fail?
netstat -an              Show all network connections
```

---

## File System Commands

### Directory Listing

```
dir                       Current directory
dir C:\                   Specific directory
dir /s                    Recursive (all subfolders)
dir /ah                   Show hidden files
dir /b                    Just filenames (no sizes)
dir /o:-s                 Sort by size (largest first)
dir /o:-d                 Sort by date (newest first)
```

### Directory Navigation

```
cd C:\Users\Username      Change to folder
cd ..                     Go to parent folder
cd \                      Go to root of drive
d:                        Change to D: drive
pwd                       Show current directory
```

### Directory Tree

```
tree C:\Users             Show folder structure
tree /d                   Just folders (no files)
tree /a                   ASCII art output
tree /L                   Limit depth
```

### File Information

```
dir filename.txt          Details about one file
attrib filename           File attributes
attrib +h filename        Hide a file
attrib -h filename        Unhide a file
```

### File Operations

```
type filename.txt         Read file contents
copy source destination   Copy file
move source destination   Move file
del filename              Delete file
```

---

## Process & Task Management

### List Processes

```
tasklist                  All processes
tasklist /v              Detailed process info
tasklist /svc            Processes with services
```

### Filter Processes

```
tasklist /fi "IMAGENAME eq chrome.exe"       Find Chrome
tasklist /fi "MEMUSAGE gt 100000"           Processes >100MB
tasklist /fi "SESSION eq 0"                 Background services
tasklist /fi "STATUS eq running"            Running processes
```

### Process Details

```
tasklist /v | find "chrome"                 Chrome memory usage
tasklist /v | sort /+30                     Sort by CPU (col 30)
```

### Kill Process

```
taskkill /im chrome.exe                 Kill by name
taskkill /pid 1234                     Kill by process ID
taskkill /im chrome.exe /f             Force kill
taskkill /s computername /im process    Kill remote process
```

### Service Commands

```
tasklist /svc                   Show services running
net start servicename           Start service
net stop servicename            Stop service
sc query servicename            Get service info
```

---

## Common Flags Reference

| Flag | Meaning | Example |
|------|---------|---------|
| /? | Help | systeminfo /? |
| /s | Remote system | systeminfo /s computer1 |
| /u | Username | /s comp1 /u domain\user |
| /p | Password | /s comp1 /u user /p pass |
| /v | Verbose (detailed) | tasklist /v |
| /fi | Filter | tasklist /fi "IMAGENAME eq x" |
| /all | Show all | ipconfig /all |
| /f | Force | taskkill /f /pid 1234 |
| /svc | Services | tasklist /svc |

---

## Piping & Filtering

### Pipe (|) Basics

```
command | find "text"                Find lines containing text
systeminfo | find "OS"               Find OS info only
tasklist | find "chrome"             Find Chrome process
dir | find ".txt"                    Find .txt files
```

### Redirect to File

```
systeminfo > systeminfo.txt          Save output to file
tasklist /v > processes.txt          Save processes to file
ipconfig /all >> network.txt          Append to file
```

### Count Results

```
tasklist /v | find /c "chrome"       How many Chrome processes?
dir /s | find /c ".txt"              How many .txt files?
```

---

## Quick Troubleshooting Commands

### Network Troubleshooting

```
ipconfig /all                        What's my IP?
route print                          How do I reach destinations?
ping google.com                      Can I reach internet?
nslookup google.com                  Does DNS work?
tracert google.com                   Where does connection fail?
netstat -an                          Who am I connected to?
```

### Process Troubleshooting

```
tasklist /v                          What's running?
tasklist /fi "MEMUSAGE gt 1000000"  What's using lots of RAM?
tasklist /v | sort /+30             What's using CPU?
taskkill /im processname /f          Kill stuck process
```

### System Troubleshooting

```
systeminfo | find "Boot"             When did system start?
systeminfo | find "OS"               What OS version?
ipconfig /renew                      Get new IP (fix network)
tasklist /svc                        What services running?
```

---

## Essential Command Examples

### Scenario 1: Can't Reach Server

```
First: What's my IP?
ipconfig | find "IPv4"

Second: Can I reach gateway?
ping 192.168.1.1

Third: Can I reach server?
ping serverip

Fourth: Where does path fail?
tracert serverip

Fifth: Does DNS work?
nslookup servername
```

### Scenario 2: System Running Slow

```
Show processes using most CPU/RAM:
tasklist /v

Find specific process:
tasklist /fi "IMAGENAME eq chrome.exe"

Kill it:
taskkill /im chrome.exe /f

Verify it's gone:
tasklist | find "chrome"
```

### Scenario 3: Need System Information

```
Everything:
systeminfo

Just OS:
systeminfo | find "OS"

Just memory:
systeminfo | find "Memory"

Save to file:
systeminfo > system_info.txt
```

### Scenario 4: Inventory Multiple Systems

```
Query one:
systeminfo /s server1

Query multiple (script):
FOR %%i IN (server1,server2,server3) DO systeminfo /s %%i >> results.txt

Result: All servers' info in results.txt
```

---

## Parameter Syntax

### Basic Pattern

```
command [what] [parameters] [/flag] [/flag value]

Examples:
systeminfo /s computer1
ipconfig /all
tasklist /fi "IMAGENAME eq name.exe"
dir /s /o:-d
```

### Quotes

When value contains spaces or special characters, use quotes:

```
tasklist /fi "IMAGENAME eq my program.exe"
```

### Case Insensitivity

Windows CLI is case-insensitive:

```
SYSTEMINFO = systeminfo = SyStEmInFo ✓ All work
```

---

## File Path Shortcuts

```
.         Current directory
..        Parent directory
C:        C: drive
C:\       Root of C: drive
~         User home (PowerShell)
%temp%    Windows temp folder
```

---

## Output Formats

### List Format (Readable)

```
tasklist /fo list
```

**Output:**
```
Image Name: chrome.exe
PID: 1234
Session Name: Console
...
```

### Table Format (Default)

```
tasklist
```

**Output (columns):**
```
Image Name    PID    Session    Memory
chrome.exe    1234   Console    456789
```

### CSV Format (Data)

```
tasklist /fo csv
```

**Output:**
```
"Image Name","PID","Session Name"
"chrome.exe","1234","Console"
```

---

## Tips & Tricks

### Redirect to Clipboard (PowerShell)

```
systeminfo | clip
```
(Copies to clipboard for pasting)

### Run as Administrator

```
Right-click Command Prompt
Select "Run as administrator"
```

### History (Previous Commands)

```
Press Up arrow key       Previous command
Press Down arrow key     Next command
F7                       Show command history
```

### Clear Screen

```
cls
```

### Exit Command Prompt

```
exit
```

---

## Common Mistakes & Fixes

### Mistake: Forgot /all flag

```
❌ ipconfig
→ Shows only basic info

✅ ipconfig /all
→ Shows everything including MAC, DHCP, DNS
```

### Mistake: Wrong quote style

```
❌ tasklist /fi 'IMAGENAME eq chrome.exe'  (single quotes)
✓ tasklist /fi "IMAGENAME eq chrome.exe"   (double quotes)
```

### Mistake: Spaces in filter

```
❌ tasklist /fi IMAGENAME eq my program.exe
→ Fails (space without quotes)

✓ tasklist /fi "IMAGENAME eq my program.exe"
→ Works (space inside quotes)
```

### Mistake: Forgot /f flag

```
taskkill /im stuck.exe
→ Asks to confirm

taskkill /im stuck.exe /f
→ Force kill immediately (no prompt)
```

---

## Keyboard Shortcuts

```
Ctrl+C          Stop running command
Ctrl+Z          Pause command
Tab             Auto-complete filename
F3              Repeat last command
F7              Show command history
Ctrl+Home       Clear screen (alt to cls)
```

---

## Best Practices

✅ **DO:**
- Use /? to get help
- Filter output with find when it's long
- Save important queries to file
- Use /s for remote queries
- Test commands on non-critical systems first

❌ **DON'T:**
- Guess at parameters (check /? instead)
- Run untested commands on production
- Kill unknown processes without researching
- Ignore error messages (they tell you what failed)

---

## Quick Command Lookup by Task

| Task | Command |
|------|---------|
| What's my IP? | ipconfig \| find "IPv4" |
| What's my gateway? | ipconfig \| find "Gateway" |
| Can I reach server? | ping serverip |
| What processes running? | tasklist /v |
| What's using RAM? | tasklist /fi "MEMUSAGE gt 100000" |
| Kill frozen process? | taskkill /im processname /f |
| What's my OS? | systeminfo \| find "OS" |
| Save system info? | systeminfo > info.txt |
| List files in folder? | dir C:\folder |
| Go to folder? | cd C:\folder |
| Create folder? | mkdir newfolder |
| Delete file? | del filename |
| Find file type? | dir /s \*.txt |
| Check routing? | route print |
| Check DNS? | nslookup google.com |

---

## Command Quick Reference Table

| Command | Primary Use | Frequency |
|---------|------------|-----------|
| systeminfo | System details | Daily |
| ipconfig | Network config | Daily |
| tasklist | Process list | Daily |
| taskkill | Kill process | Common |
| dir | File listing | Common |
| cd | Navigate | Common |
| ping | Connectivity | Troubleshooting |
| nslookup | DNS | Troubleshooting |
| route print | Routing | Troubleshooting |
| arp -a | MAC lookup | Occasional |
| netstat | Connections | Troubleshooting |

---

**Print this page or bookmark for quick reference during troubleshooting.**

**Room 1 mastered. Ready for Room 2 and advanced Windows CLI skills.**
