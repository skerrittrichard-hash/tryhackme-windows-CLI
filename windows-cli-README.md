# TryHackMe Windows Command Line - Room 1: Command Line Basics

Complete documentation of TryHackMe's Windows Command Line Room 1. Practical guide to essential Windows command-line commands for system administration and troubleshooting.

**Status:** ✅ Complete - All 5 tasks documented

---

## Room Overview

**TryHackMe Room:** Windows Command Line - Room 1: Command Line Basics  
**Duration:** ~45-60 minutes (lab + hands-on practice)  
**Difficulty:** Beginner-Intermediate  
**Hands-On Labs:** Yes (command execution, system queries, network diagnostics)

## What This Room Covers

Windows Command Line Basics teaches the essential command-line tools for system administration. Unlike GUI-based learning, CLI skills are faster, more powerful, and essential for IT professionals.

### Why Learn Command Line?

**Speed:** One command vs clicking through 5 menus
```
GUI: Settings → System → About → Device specifications → See OS
CLI: systeminfo | find "OS Name"
```

**Power:** Automate tasks, script repeatable actions
```
GUI: Manually check 100 computers (hours)
CLI: Script queries all 100 at once (seconds)
```

**Remote Management:** Can't see GUI on remote server
```
GUI: Can't work (no display)
CLI: Works perfectly from anywhere
```

**Professional:** What sysadmins actually use daily

### Topics Covered

1. **Introduction & Command Line Basics**
   - Command prompt interface
   - How to enter and execute commands
   - Command syntax and parameters
   - Getting help (command /?)
   - SSH connection to remote systems
   - Basic command structure

2. **Basic System Information**
   - systeminfo - Detailed system information
   - OS version and build number
   - Hardware configuration
   - Processor details
   - RAM information
   - System boot time
   - Hardware inventory

3. **Network & Routing Commands**
   - ipconfig - IP configuration
   - ipconfig /all - Detailed network info
   - route print - Routing table
   - arp -a - Address resolution protocol
   - getmac - MAC address lookup
   - Network adapter details
   - Gateway and DNS information
   - Network metrics

4. **File System & File Operations**
   - dir - Directory listing
   - cd - Change directory
   - Tree - Directory tree display
   - File size and timestamps
   - File attributes
   - Directory structure
   - File information retrieval
   - Path navigation

5. **Task & Process Management**
   - tasklist - List running processes
   - tasklist /v - Detailed process info
   - tasklist /fi - Filter processes
   - Process ID (PID)
   - Memory usage per process
   - Process status
   - CPU information
   - Service processes

---

## Practical Skills You'll Learn

### Information Gathering (Reconnaissance)

```
Get system details:
systeminfo

Get network configuration:
ipconfig /all

Get routing information:
route print

Get all running processes:
tasklist /v
```

**Real-world use:** Audit system, verify configuration, understand baseline.

### Troubleshooting

```
Process hogging CPU? Find it:
tasklist /fi "STATUS eq running"

Network not working? Check gateway:
ipconfig | find "Default Gateway"

Routing issues? View route table:
route print
```

**Real-world use:** Diagnose problems, identify culprits, fix issues.

### System Administration

```
Remote query system:
systeminfo /s remotecomputer

Check all processes on server:
tasklist /v /s servername

List all network adapters:
ipconfig /all

Verify OS version:
systeminfo | find "OS"
```

**Real-world use:** Manage multiple systems, verify configurations, compliance checks.

---

## Essential Commands Quick Reference

| Command | Purpose | Example |
|---------|---------|---------|
| systeminfo | System details | systeminfo |
| ipconfig | Network config | ipconfig /all |
| route print | Routing table | route print |
| arp -a | MAC addresses | arp -a |
| tasklist | Running processes | tasklist /v |
| tasklist /fi | Filter processes | tasklist /fi "IMAGENAME eq chrome.exe" |
| dir | Directory listing | dir C:\ |
| tree | Directory tree | tree C:\Users |
| getmac | MAC address | getmac |
| nslookup | DNS lookup | nslookup google.com |

---

## Room Structure

### Task 1: Introduction & Command Line Basics
**Learn:** How command line works, basic syntax, getting help

**Key concepts:**
- Command structure (command parameter /flag)
- Case-insensitivity on Windows
- /? for help on any command
- Remote access via SSH
- AttackBox connection

### Task 2: Basic System Information
**Learn:** Query system details without GUI

**Key commands:**
- `systeminfo` - Everything about your system
- `systeminfo | find "Windows"` - Filter for specific info
- `wmic os get` - WMI system queries
- `ver` - Just the OS version

**Sysadmin use:** Inventory systems, verify patches, check hardware.

### Task 3: Network & Routing Commands
**Learn:** Understand network configuration from command line

**Key commands:**
- `ipconfig` - Quick IP info
- `ipconfig /all` - Detailed network config
- `route print` - Routing table
- `arp -a` - ARP table (MAC/IP mapping)
- `getmac` - MAC address only
- `nslookup host` - DNS lookup

**Sysadmin use:** Troubleshoot network issues, verify gateway, check DNS.

### Task 4: File System & File Operations
**Learn:** Navigate and query file system from CLI

**Key commands:**
- `dir` - List directory (DOS style)
- `cd` - Change directory
- `tree` - Tree view of directories
- `dir /s` - Recursive listing
- `dir /ah` - Show hidden files

**Sysadmin use:** Navigate remote systems, verify permissions, find files.

### Task 5: Task & Process Management
**Learn:** Monitor and manage running processes

**Key commands:**
- `tasklist` - List all processes
- `tasklist /v` - Detailed process info
- `tasklist /fi` - Filter processes
- `tasklist /fi "IMAGENAME eq name.exe"` - Find specific process
- `Get-Process` (PowerShell) - Modern alternative

**Sysadmin use:** Monitor system health, find runaway processes, troubleshoot.

---

## Real-World Scenarios

### Scenario 1: "System is running slow"

```
1. Open Command Prompt
2. tasklist /v
3. Sort by CPU or Memory column
4. Identify offending process
5. taskkill /im processname.exe
6. System responsive again
```

### Scenario 2: "Network is down"

```
1. ipconfig /all
2. Check: IP address, Subnet mask, Default gateway
3. route print
4. Verify default route exists
5. nslookup google.com
6. Check if DNS resolving
7. Identify problem (IP/Gateway/DNS)
```

### Scenario 3: "Inventory all company computers"

```
Create script to query 100 systems:
for %%i in (comp1,comp2,comp3...) do (
  systeminfo /s %%i >> inventory.txt
)

Result: Inventory of all systems in minutes
```

### Scenario 4: "Security audit - find suspicious processes"

```
tasklist /v > processes.txt
Review for: Unknown processes, unusual parent processes, high memory usage
Find: Malware typically hides in System32 or uses legitimate-sounding names
```

---

## Comparison: GUI vs CLI

### Task: Check if Windows is up to date

**GUI:**
1. Settings
2. System
3. About
4. Check for updates
5. View update history
6. See last update date

**CLI:**
```
systeminfo | find "System Boot Time"
Get-HotFix -Latest
```

**Winner:** CLI - 1 command vs 6 clicks

### Task: Find which process is using all RAM

**GUI:**
1. Task Manager
2. Processes tab
3. Click Memory column
4. Sort descending
5. Find top process

**CLI:**
```
tasklist /v | find /i "process"
```

**Winner:** CLI - Can filter, script, send to file, remote query

---

## Security+ Alignment

This room supports Security+ knowledge in:

- **System Administration:** Command-line basics for managing systems
- **Incident Response:** Quickly query systems for forensics
- **Vulnerability Assessment:** Identify outdated OS versions, missing patches
- **Network Troubleshooting:** Diagnose network connectivity issues
- **Process Monitoring:** Identify suspicious processes running

---

## Career Relevance

**Jobs that use these skills:**

- System Administrator
- IT Operations Specialist
- Support Technician
- Network Administrator
- Security Administrator
- IT Auditor
- Help Desk (escalations)

**Interview questions:**

*"How would you query system information on 50 computers?"*
- Answer: Script using systeminfo /s with computer list, output to file

*"System running slow - how would you troubleshoot?"*
- Answer: tasklist /v to see processes, sort by CPU/Memory, identify culprit

*"How do you check network configuration?"*
- Answer: ipconfig /all shows everything - IP, gateway, DNS, adapters

---

## Hands-On Practice

### Exercise 1: System Inventory
```
1. Open Command Prompt
2. Type: systeminfo
3. Record: OS version, processor, RAM, build number
4. Type: systeminfo /s remotename (if you have access)
```

### Exercise 2: Network Troubleshooting
```
1. Type: ipconfig /all
2. Identify: Your IP, subnet mask, default gateway, DNS
3. Type: nslookup google.com
4. Verify DNS resolution works
```

### Exercise 3: Process Monitoring
```
1. Type: tasklist /v
2. Identify: Top processes by CPU, RAM usage
3. Type: tasklist /fi "IMAGENAME eq chrome.exe"
4. Find specific process
```

### Exercise 4: File System Navigation
```
1. Type: dir C:\Users
2. Type: tree C:\Users /L /A
3. Explore directory structure
4. Navigate using cd commands
```

---

## Tips & Tricks

### Pipe for filtering
```
systeminfo | find "OS Name"
```
Shows only lines containing "OS Name"

### /v flag for verbose (detailed)
```
tasklist /v
```
Shows columns you can sort by

### /s for remote systems
```
systeminfo /s computername
```
Query remote computer

### Wildcards for flexibility
```
tasklist /fi "IMAGENAME eq *chrome*"
```
Finds any process with "chrome" in name

### Output to file
```
systeminfo > systeminfo.txt
```
Saves output to file for review

---

## Comparison to Windows Fundamentals

This room complements Windows Fundamentals by providing **CLI-based** approach to the same tasks:

| Task | Windows Fundamentals | Windows Command Line Room 1 |
|------|-----|-----|
| System info | GUI (Settings) | CLI (systeminfo) |
| Network config | GUI (Settings) | CLI (ipconfig) |
| Process list | GUI (Task Manager) | CLI (tasklist) |
| File navigation | GUI (File Explorer) | CLI (dir, cd, tree) |

**Progression:** GUI fundamentals → CLI practical skills

---

## Series Structure

```
Windows Command Line Series

Room 1: Command Line Basics (what you're doing now)
├─ System information (systeminfo)
├─ Network commands (ipconfig, route, nslookup)
├─ File operations (dir, cd, tree)
└─ Process management (tasklist, taskkill)

Room 2+: (To be documented)
└─ Advanced topics
```

---

## Next Steps

After mastering command-line basics:

1. **PowerShell Advanced** - Scripting and automation
2. **Active Directory (CLI)** - Manage domain users/computers from command line
3. **Windows Server** - Apply CLI skills to server administration
4. **Batch Scripting** - Automate tasks with batch scripts
5. **System Administration** - Real-world troubleshooting

---

## Summary

Windows Command Line Basics teaches you:

✅ Query system information without GUI  
✅ Troubleshoot network issues from CLI  
✅ Monitor processes and performance  
✅ Navigate file system remotely  
✅ Gather system inventory at scale  
✅ Diagnose problems quickly  
✅ Automate repetitive tasks  
✅ Work on remote systems (no GUI)

**Skills learned:** ~15-20 essential commands covering system admin, networking, and troubleshooting.

**Career impact:** CLI skills are essential for any IT role. Separates beginners from professionals.

---

**Completed:** February 2026  
**Room Link:** https://tryhackme.com/room/windowscmdline  
**Time Investment:** ~60 minutes  
**Skill Level After:** Intermediate command-line user  

**Repository:** tryhackme-windows-command-line  
**Structure:** room-01-README.md, room-01-notes.md, room-01-commands-cheatsheet.md, room-01-screenshots/ (5 images)

**Series Status:** Room 1 of Windows Command Line series complete. Room 2+ pending.

---

**Windows Command Line - Room 1 complete. Ready for Room 2 and advanced CLI skills.**
