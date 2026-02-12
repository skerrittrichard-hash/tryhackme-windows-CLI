# TryHackMe - Windows Command Line: Room 2 - PowerShell

**Course:** Windows Command Line Series  
**Room:** PowerShell Fundamentals  
**Platform:** TryHackMe  
**Difficulty:** Beginner  
**Focus:** PowerShell scripting, automation, and system administration

---

## Overview

Room 2 transitions from traditional Windows Command Prompt (cmd.exe) to **PowerShell**, Microsoft's modern task automation and configuration management framework. Unlike cmd.exe's text-based approach, PowerShell is object-oriented and built on the .NET Framework, providing powerful capabilities for system administrators.

This room covers PowerShell fundamentals including cmdlet syntax, file system navigation, system information gathering, process management, and basic scripting—essential skills for Windows system administration and automation.

---

## Learning Objectives

By completing this room, you will learn:

✅ **PowerShell Architecture:**
- Understand PowerShell as an object-oriented shell (vs text-based)
- Learn the .NET Framework integration
- Distinguish between Windows PowerShell and PowerShell Core

✅ **Cmdlet Fundamentals:**
- Master the Verb-Noun naming convention
- Discover available cmdlets with Get-Command
- Access documentation with Get-Help
- Install and manage PowerShell modules

✅ **File System Operations:**
- Navigate directories with Get-ChildItem
- Filter files by type, size, and properties
- Use pipelines to chain commands
- Sort and measure file system objects

✅ **System Administration:**
- Retrieve comprehensive system information
- Monitor network adapters and connections
- Manage Windows services and processes
- Access event logs for troubleshooting

✅ **Automation & Scripting:**
- Create and execute PowerShell scripts (.ps1)
- Use Invoke-Command for remote execution
- Automate repetitive administrative tasks

---

## Task Breakdown

### **Task 1: Introduction to PowerShell**
- PowerShell overview and purpose
- Learning objectives
- Room prerequisites (Windows Command Line basics)
- Cmdlets, pipelines, and object manipulation
- Variables, loops, and filtering

### **Task 2: What is PowerShell?**
- PowerShell definition (automation framework + shell)
- Object-oriented vs text-based processing
- Brief history: Windows PowerShell → PowerShell Core
- Cross-platform capabilities (Windows, Linux, macOS)
- .NET Framework integration
- Version evolution (1.0 through 7.x)

### **Task 3: PowerShell Basics**
**Part 1 - Launching & Syntax:**
- Multiple launch methods (Start menu, Terminal, Run dialog)
- Verb-Noun cmdlet structure
- Basic cmdlets: Get-Command, Get-Help, Get-ChildItem, Set-Location, Clear-Host

**Part 2 - Cmdlet Discovery:**
- Get-Command with filters and wildcards
- Get-Help for documentation and examples
- Get-Date for working with dates/times
- PowerShell Gallery and community modules

**Part 3 - Module Management:**
- Get-Module for listing installed/available modules
- Module repository sources (PSGallery)
- Installation paths and locations

### **Task 4: Navigating the File System and Working with Files**
**Part 1 - Basic Navigation:**
- Get-ChildItem for listing files/directories
- Parameters: `-Path`, `-Recurse`, `-Filter`, `-Name`, `-Force`
- Filtering by file type and attributes
- Showing hidden files and system folders

**Part 2 - Advanced Operations:**
- Combining parameters for complex queries
- Sort-Object for organizing output
- Select-Object for choosing specific properties
- Measure-Object for statistics and counting
- Pipeline usage for chaining commands
- Finding largest files and calculating directory sizes

### **Task 5: System and Network Information**
- **Get-ComputerInfo:** Comprehensive system details (OS, hardware, BIOS)
- **Get-LocalUser:** User account management and properties
- **Get-NetAdapter / Get-NetIPAddress:** Network interface configuration
- **Get-NetNeighbor:** ARP cache and neighbor discovery
- Network troubleshooting techniques

### **Task 6: Managing Processes and Services**
- **Get-Service:** Windows service status and management
- **Get-NetTCPConnection:** Active network connections and ports
- **Get-EventLog:** System, Application, and Security event logs
- **Get-Process:** Running processes, memory, and CPU usage
- Filtering events by time and type
- System monitoring and troubleshooting

### **Task 7: Scripting**
- Creating PowerShell scripts (.ps1 files)
- Script organization and structure
- **Invoke-Command:** Remote command execution
- Automation use cases (bulk operations, configuration management)
- Best practices: documentation, error handling, security

---

## Key Takeaways

### **PowerShell vs cmd.exe:**
| Feature | cmd.exe | PowerShell |
|---------|---------|------------|
| **Data Type** | Text-based | Object-oriented |
| **Output** | Plain text | Structured objects |
| **Scripting** | Batch files | .ps1 scripts |
| **Commands** | Limited built-ins | Extensive cmdlets |
| **Automation** | Basic | Advanced |

### **Essential Cmdlet Patterns:**
- **Discovery:** `Get-Command`, `Get-Help`, `Get-Module`
- **File System:** `Get-ChildItem`, `Select-Object`, `Sort-Object`, `Measure-Object`
- **System Info:** `Get-ComputerInfo`, `Get-LocalUser`
- **Networking:** `Get-NetAdapter`, `Get-NetIPAddress`, `Get-NetTCPConnection`
- **Services/Processes:** `Get-Service`, `Get-Process`, `Get-EventLog`
- **Remote Execution:** `Invoke-Command`

### **Pipeline Power:**
PowerShell's pipeline passes **objects** (not text), enabling powerful command chaining:
```powershell
Get-ChildItem -Recurse -Filter *.log | 
  Sort-Object Length -Descending | 
  Select-Object -First 10 | 
  Measure-Object -Property Length -Sum
```

---

## Practical Applications for System Administrators

### **Daily Operations:**
1. **System Health Checks:**
   - Verify service status across servers
   - Monitor disk space and performance
   - Check network connectivity and configuration

2. **User Management:**
   - List and audit local user accounts
   - Verify account properties and security settings
   - Bulk user account operations

3. **Troubleshooting:**
   - Search event logs for errors
   - Identify resource-intensive processes
   - Trace network connection issues

### **Automation Opportunities:**
- **Scheduled Tasks:** Automated backups, log rotations, health checks
- **Remote Administration:** Manage multiple servers from single console
- **Configuration Management:** Deploy settings across infrastructure
- **Reporting:** Generate system status reports automatically

### **Career Relevance:**
- **Essential for Windows Sysadmin roles**
- Required skill for Azure/Microsoft 365 administration
- Foundation for DevOps and automation careers
- Commonly tested in CompTIA Server+, MCSA, and Azure certifications

---

## Repository Structure

```
tryhackme-windows-command-line/
├── room-01-README.md (cmd.exe basics)
├── room-01-notes.md
├── room-01-commands-cheatsheet.md
├── room-01-screenshots/
│
├── room-02-README.md (PowerShell - this file)
├── room-02-notes.md (detailed documentation)
├── room-02-commands-cheatsheet.md (quick reference)
└── room-02-screenshots/
    ├── room-02-task-01-introduction-to-powershell.png
    ├── room-02-task-02-what-is-powershell.png
    ├── room-02-task-03-powershell-basics-part1.png
    ├── room-02-task-03-powershell-basics-part2.png
    ├── room-02-task-03-powershell-basics-part3.png
    ├── room-02-task-04-navigating-file-system-and-working-with-files.png
    ├── room-02-task-04-navigating-file-system-part2.png
    ├── room-02-task-05-system-and-network-information.png
    ├── room-02-task-06-managing-processes-and-services.png
    └── room-02-task-07-scripting.png
```

---

## Progression Path

**Room 1 (cmd.exe)** → **Room 2 (PowerShell)** → Advanced PowerShell Scripting

After mastering these fundamentals:
- Explore PowerShell remoting (PSRemoting)
- Learn advanced scripting (functions, modules, error handling)
- Study Active Directory management with PowerShell
- Practice automation scenarios for real-world IT environments

---

## Notes

- **Completion Date:** February 2026
- **Platform:** TryHackMe
- **Documentation:** Hands-on practical exercises with screenshots
- **AI Assistance:** Documentation created with Claude (Anthropic) - see commit messages for transparency

---

**PowerShell is the modern standard for Windows administration. Master it = unlock automation capabilities essential for IT operations roles.**
