# TryHackMe Windows Command Line - Room 1: Detailed Notes

Comprehensive explanations of Windows command-line commands and their practical applications in Room 1.

---

## Task 1: Introduction & Command Line Basics

### What is Command Prompt?

Command Prompt (cmd.exe) is the Windows command-line interface. It allows you to:
- Execute commands directly
- Query system information
- Manage files and directories
- Control services and processes
- Automate tasks with scripts

### Command Syntax

**Basic structure:**
```
command [parameters] [/flags]
```

**Example:**
```
systeminfo /s computername /u username /p password
│         │  │   │           │  │         │  │
command   param flag  value   flag value   flag value
```

**Case insensitivity:**
Windows commands are NOT case-sensitive.
```
SYSTEMINFO = systeminfo = SyStEmInFo (all work the same)
```

### Getting Help

**Every command has help:**
```
command /?
```

**Examples:**
```
systeminfo /?          Show help for systeminfo
ipconfig /?            Show help for ipconfig
tasklist /?            Show help for tasklist
```

**Help shows:**
- Description of command
- All available parameters
- Usage examples
- Common flags

### Common Flags

**Flags modify command behavior:**

| Flag | Meaning | Example |
|------|---------|---------|
| /? | Help | systeminfo /? |
| /s | Specifies system | systeminfo /s computer1 |
| /u | Username | systeminfo /s comp1 /u username |
| /p | Password | systeminfo /s comp1 /u user /p pass |
| /v | Verbose (detailed) | tasklist /v |
| /fi | Filter | tasklist /fi "STATUS eq running" |
| /fo | Format output | tasklist /fo list |

### Remote Execution

**Query remote system:**
```
systeminfo /s computername
```

**With credentials:**
```
systeminfo /s computername /u domain\username /p password
```

**Real-world use:** Query 100 servers without visiting each one.

---

## Task 2: Basic System Information

### systeminfo - The Master Command

**What it does:**
Displays comprehensive information about a computer and its operating system.

**Basic usage:**
```
systeminfo
```

**Output includes:**
- OS name and version
- Build number
- System boot time
- Hardware manufacturer
- Processor name and speed
- RAM (physical memory)
- System locale
- Time zone
- Network configuration

### Reading systeminfo Output

**Key lines to look for:**

```
OS Name: Microsoft Windows 10 Professional
→ What OS is installed

OS Version: 10.0.19044 Build 19044
→ Exact version (used for patching decisions)

System Boot Time: 2/11/2026, 9:15:23 AM
→ When system last started (uptime indicator)

Processor: Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz
→ Processor speed and model

Total Physical Memory: 16384 MB (16 GB)
→ How much RAM installed

Available Physical Memory: 8192 MB (8 GB)
→ How much RAM currently free
```

### Filtering systeminfo

**Get only OS name:**
```
systeminfo | find "OS Name"
```

**Get only memory:**
```
systeminfo | find "Physical Memory"
```

**How it works:**
- `systeminfo` runs and produces output
- `|` (pipe) sends that output to the next command
- `find "text"` searches for lines containing that text
- Only matching lines displayed

### Remote systeminfo

**Query another computer:**
```
systeminfo /s othermachine
```

**Real-world scenario:**
Manager asks: "What OS version is on server5?"
```
systeminfo /s server5 | find "OS Version"
→ Instant answer without logging into server5
```

### WMI Commands (Alternative)

**Modern way to query system:**
```
wmic os get caption,version,buildnumber
```

**Advantage:**
- Faster
- More flexible
- Can output to CSV
- Scriptable

**Example:**
```
wmic os get caption,version,buildnumber /format:csv > osinfo.csv
→ Saves OS info from all systems to CSV file
```

---

## Task 3: Network & Routing Commands

### ipconfig - Network Configuration

**What it shows:**
IP address, subnet mask, default gateway, DNS servers for all network adapters.

**Basic usage:**
```
ipconfig
```

**Output:**
```
Ethernet adapter Ethernet:
   Connection-specific DNS Suffix: company.com
   IPv4 Address: 192.168.1.100
   Subnet Mask: 255.255.255.0
   Default Gateway: 192.168.1.1
```

**Detailed version:**
```
ipconfig /all
```

**Shows additionally:**
- MAC address (Physical Address)
- DHCP server
- Lease obtained/expires times
- DNS servers
- All adapters (even disabled ones)

### Troubleshooting with ipconfig

**Problem: No internet connection**

```
Step 1: Check your IP
ipconfig | find "IPv4 Address"
→ If "169.254.x.x" = DHCP failed, no IP assigned

Step 2: Check gateway
ipconfig | find "Default Gateway"
→ If blank = no route to internet

Step 3: If no IP, renew DHCP
ipconfig /renew
→ Request new IP from DHCP server

Step 4: If still broken, release and renew
ipconfig /release
ipconfig /renew
→ Sometimes fixes stubborn issues
```

### route print - Routing Table

**What it shows:**
How packets are routed to different networks.

**View routing table:**
```
route print
```

**Output columns:**

| Column | Meaning | Example |
|--------|---------|---------|
| Destination | Target network | 192.168.1.0 |
| Netmask | Which hosts in network | 255.255.255.0 |
| Gateway | Router IP | 192.168.1.1 |
| Metric | Cost/priority | 10 (lower=better) |

**Common routes:**

```
0.0.0.0 (Default route)
→ Where packets go if no other route matches
→ Default Gateway is this route

127.0.0.1 (Localhost)
→ Your own computer

224.0.0.0 (Multicast)
→ Special network for group communication
```

### arp -a - Address Resolution Protocol

**What it shows:**
Mapping of IP addresses to MAC addresses on local network.

**View ARP table:**
```
arp -a
```

**Output:**

```
Internet Address: 192.168.1.1
Physical Address: aa-bb-cc-dd-ee-ff
Type: dynamic
```

**Real-world use:**
- Find MAC address of device
- Troubleshoot network connectivity
- Identify devices on network

### getmac - MAC Address Lookup

**Show MAC address only:**
```
getmac
```

**Show MAC of remote computer:**
```
getmac /s computername
```

**Real-world use:**
- Network device registration
- Device identification
- Troubleshooting MAC-based issues

### nslookup - DNS Lookup

**Resolve hostname to IP:**
```
nslookup google.com
```

**Output:**

```
Name: google.com
Addresses: 142.251.41.14
```

**Troubleshooting DNS:**

```
nslookup google.com
→ If fails = DNS server unreachable

nslookup google.com 8.8.8.8
→ Query specific DNS server (Google's)
```

### Network Troubleshooting Flow

**Internet down?**

```
1. ipconfig /all
   → Do I have an IP? Is it valid (not 169.254)?

2. route print | find "0.0.0.0"
   → Is there a default route to gateway?

3. ping 192.168.1.1 (your gateway)
   → Can I reach the gateway?

4. nslookup google.com
   → Is DNS working?

5. tracert google.com
   → Where does connection fail?

Result: Identify if problem is local, gateway, DNS, or ISP
```

---

## Task 4: File System & File Operations

### dir - Directory Listing

**List current directory:**
```
dir
```

**List specific directory:**
```
dir C:\Users
```

**Important columns:**

| Column | Meaning |
|--------|---------|
| Date | When file was created/modified |
| Time | Time of last modification |
| Size | File size in bytes |
| Name | File or folder name |
| `<DIR>` | This is a folder, not file |

### Useful dir Flags

**Show hidden files:**
```
dir /ah
```

**Recursive (all subfolders):**
```
dir /s
```

**Just filenames:**
```
dir /b
```

**Large files first:**
```
dir /s /o:-s
```

**Time-ordered (newest first):**
```
dir /o:-d
```

### cd - Change Directory

**Go to folder:**
```
cd C:\Users\Username\Documents
```

**Go to parent folder:**
```
cd ..
```

**Go to root of drive:**
```
cd \
```

**Change drive:**
```
d:
```
(switches to D: drive)

### tree - Directory Tree

**Show folder structure:**
```
tree C:\Users
```

**Without files (just folders):**
```
tree /d
```

**Graphical output:**
```
tree /a C:\Users
```

**Real-world use:**
Understand directory structure of unfamiliar system.

### File Information

**Get file size:**
```
dir myfile.txt
```

**Get file attributes:**
```
attrib myfile.txt
```

**Example output:**
```
A       Archived (not backed up yet)
R       Read-only (can't edit)
H       Hidden (not normally visible)
S       System (Windows system file)
```

---

## Task 5: Task & Process Management

### tasklist - List Processes

**Show all running processes:**
```
tasklist
```

**Output:**

```
Image Name                     PID Session Name        Session#    Memory Usage
svchost.exe                    1234 Services           0          2,048 KB
chrome.exe                     5678 Console            1          456,789 KB
notepad.exe                    9012 Console            1          1,024 KB
```

### tasklist /v - Detailed View

**Show detailed process info:**
```
tasklist /v
```

**Additional columns:**

| Column | Meaning |
|--------|---------|
| PID | Process ID (unique identifier) |
| Session Name | Services (background) or Console (user) |
| CPU Time | How much CPU this process used |
| Window Title | What the window is called |
| Status | Whether running normally |
| User Name | Which user started it |
| Memory | RAM used by process |

### Filtering with /fi

**Find specific process:**
```
tasklist /fi "IMAGENAME eq chrome.exe"
```

**Filter syntax:**
```
tasklist /fi "OPERATOR value"
```

**Common operators:**

| Operator | Meaning | Example |
|----------|---------|---------|
| eq | equals | IMAGENAME eq chrome.exe |
| ne | not equals | IMAGENAME ne svchost.exe |
| gt | greater than | MEMUSAGE gt 100000 |
| lt | less than | MEMUSAGE lt 50000 |

**Useful filters:**

```
Find Chrome processes:
tasklist /fi "IMAGENAME eq chrome.exe"

Find processes using >100MB:
tasklist /fi "MEMUSAGE gt 100000"

Find services (not user apps):
tasklist /fi "SESSION eq 0"

Find user apps:
tasklist /fi "SESSION eq 1"
```

### taskkill - Kill Process

**Kill by name:**
```
taskkill /im chrome.exe
```

**Kill by PID:**
```
taskkill /pid 5678
```

**Force kill (don't ask):**
```
taskkill /im chrome.exe /f
```

**Practical example:**
```
Application frozen, won't respond

tasklist /fi "IMAGENAME eq notepad.exe"
→ Find PID: 9012

taskkill /pid 9012 /f
→ Force kill that process

Application closed, system responsive again
```

### Real-Time Process Monitoring

**Alternative to Task Manager:**
```
tasklist /v | sort /+30
```
(Sorts by CPU time column)

**Or with PowerShell (modern):**
```
Get-Process | Sort CPU -Descending | head 10
```
(Shows top 10 by CPU)

### Process Management Troubleshooting

**System slow?**

```
tasklist /v /fo csv > processes.csv
→ Export to CSV for analysis

Then look for:
- Processes with high CPU usage
- Processes with large memory usage
- Unknown processes (malware?)
```

**Service crashed?**

```
tasklist | find "servicename"
→ Is service running?

If not running:
net start servicename
→ Restart it

Check Event Viewer for error:
eventvwr.msc
```

---

## Command-Line Best Practices

### 1. Use /? for Help

Never guess at parameters. Always check help first.

```
systeminfo /?
ipconfig /?
tasklist /?
```

### 2. Filter Output with Pipe |

Too much output? Filter it.

```
tasklist /v | find "chrome"
→ Find specific process

systeminfo | find "OS"
→ Find OS information

dir /s | find ".txt"
→ Find all .txt files
```

### 3. Output to File

Redirect output for later review or sharing.

```
systeminfo > systeminfo.txt
→ Save to file

tasklist /v > processes.txt
→ Save process list
```

### 4. Use Remote Flags (/s)

Work on remote systems without RDP.

```
systeminfo /s server1
→ Query remote system

ipconfig /s server2
→ Check remote network config
```

### 5. Know Common Abbreviations

Shorthand commonly used:

| Abbrev | Meaning |
|--------|---------|
| /s | System (remote computer) |
| /u | Username |
| /p | Password |
| /v | Verbose (detailed) |
| /fi | Filter |
| /fo | Format output |
| /all | Show everything (ipconfig /all) |

---

## Comparison: CLI vs GUI

### Check OS Version

**GUI:**
Settings → System → About → Look at OS version

**CLI:**
```
systeminfo | find "OS"
```

**Time:** GUI = 10 seconds, CLI = 1 second

### Find Process Using CPU

**GUI:**
Task Manager → Processes → Click CPU column

**CLI:**
```
tasklist /v | find "high CPU" (approximate)
```

**Advantage:** CLI can be scripted and run remotely.

### Check Network Configuration

**GUI:**
Settings → Network & Internet → Advanced network settings

**CLI:**
```
ipconfig /all
```

**Advantage:** CLI shows all at once, no clicking.

---

## Troubleshooting Scenarios

### Scenario 1: "Server using all RAM"

```
tasklist /v
Sort by Memory column
Find process consuming RAM
taskkill /pid [number] /f
Memory freed
```

### Scenario 2: "Can't reach server"

```
ipconfig /all
→ Check your IP and gateway

ping serverip
→ Can you reach it?

route print
→ Is route to it configured?

tracert serverip
→ Where does it fail?

nslookup servername
→ Does DNS work?
```

### Scenario 3: "Need to audit 50 systems"

```
Create text file with 50 computer names

Script to loop through and run:
systeminfo /s computername >> results.txt

Result: Complete inventory in seconds
```

### Scenario 4: "Process keeps restarting"

```
tasklist /v | find "processname"
→ Get PID

taskkill /pid [number] /f
→ Kill it

Wait 10 seconds
tasklist /v | find "processname"
→ Did it come back?

If yes: Autostart (check Task Scheduler)
→ tasklist /svc (show services)
→ Disable auto-restart
```

---

## Summary: When to Use Command Line

**Use CLI when you need to:**
- ✅ Query multiple systems quickly
- ✅ Troubleshoot without GUI access
- ✅ Automate repetitive tasks
- ✅ Filter large amounts of data
- ✅ Work remotely
- ✅ Create audit trails

**Examples:**
- Inventory 100 servers in 5 minutes (CLI)
- Check one computer's RAM (GUI is fine)
- Schedule daily system queries (CLI script)
- One-time file search (GUI is fine)
- Monitor production servers 24/7 (CLI script)

---

**Windows Command Line - Room 1 mastered. Ready for Room 2 and practical system administration.**
