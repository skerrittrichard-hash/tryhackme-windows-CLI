# TryHackMe - Linux Shells

**Course:** Cyber Security 101 - Command Line Path  
**Room:** Linux Shells  
**Platform:** TryHackMe  
**Difficulty:** Beginner  
**Focus:** Shell fundamentals, bash scripting, and automation

---

## Overview

This room introduces Linux shells—the command-line interfaces that enable interaction with Linux operating systems. Unlike graphical interfaces (GUI) that require clicking through menus, shells provide powerful, efficient, and resource-friendly ways to manage systems through text commands.

The room covers shell basics, different shell types (Bash, Zsh, Sh), fundamental Linux commands, and bash scripting including variables, loops, and conditionals. These are essential skills for system administration, cybersecurity, and automation roles.

---

## Learning Objectives

By completing this room, you will learn:

✅ **Shell Fundamentals:**
- Understand what shells are and their role as OS interface
- Distinguish between physical and virtual shells
- Identify your current shell environment
- Navigate between different shell types

✅ **Essential Linux Commands:**
- File system navigation (ls, cd)
- File operations (cat, grep)
- System information (echo $SHELL, echo $0)
- Directory listing with options (-l, -a, -la)

✅ **Shell Types:**
- Bash (Bourne Again Shell) - most common Linux shell
- Zsh (Z Shell) - modern alternative with enhanced features
- Sh (Bourne Shell) - original Unix shell
- Compare features, syntax, and use cases

✅ **Bash Scripting:**
- Script structure and shebang (#!/bin/bash)
- Variables and user input (read command)
- Loops (for loops with ranges and iteration)
- Conditional statements (if/then/else/fi)
- Comments for documentation
- Script permissions and execution (chmod +x)

✅ **Practical Automation:**
- Create authentication scripts
- Search and filter log files
- Automate repetitive tasks
- Real-world troubleshooting scenarios

---

## Task Breakdown

### **Task 1: Introduction to Linux Shells**
- Shell definition and purpose
- Shell as interface between user and OS
- GUI vs CLI comparison
- Learning objectives overview
- Room prerequisites

### **Task 2: How to Interact with a Shell?**
- Virtual vs physical shell concepts
- Identifying current shell: `echo $0`, `echo $SHELL`
- Basic command syntax and structure

**Essential Commands:**
- `ls` - List directory contents
  - `-l` - Long format with details
  - `-a` - Show hidden files
  - `-la` - Combine both options
- `cd` - Change directory
- `cat` - Display file contents
- `grep` - Search for text/patterns in files

**Questions:**
- What is the default shell in most Linux distributions? → `bash`
- Which command utility lists directory contents? → `ls`
- Which command utility searches for anything in a file? → `grep`

### **Task 3: Types of Shells**

**Viewing Available Shells:**
```bash
cat /etc/shells      # List all installed shells
echo $SHELL          # Show default shell
echo $0              # Show current shell
```

**Common Shell Types:**

**1. Bash (Bourne Again Shell):**
- Default on most Linux distributions
- Enhanced version of original Bourne shell
- Features: command history, tab completion, scripting
- Most widely documented and supported

**2. Zsh (Z Shell):**
- Modern alternative to Bash
- Enhanced tab completion
- Better customization (themes, plugins)
- Improved scripting features
- Default shell on macOS (since Catalina)

**3. Sh (Bourne Shell):**
- Original Unix shell
- Simpler, less feature-rich
- Used for basic system scripts
- POSIX compliant

**Switching Shells:**
```bash
zsh       # Switch to Z Shell
bash      # Switch to Bash
sh        # Switch to Bourne Shell
exit      # Return to previous shell
```

**Comparison:**
| Feature | Bash | Zsh | Sh |
|---------|------|-----|----|
| Tab Completion | Basic | Advanced | Basic |
| Customization | Moderate | Extensive | Minimal |
| Scripting | Full-featured | Enhanced | Basic |
| History | Yes | Yes | Limited |
| Themes/Plugins | Limited | Extensive | None |

### **Task 4: Scripting (Part 1)**

**Script Basics:**

**The Shebang (Interpreter Declaration):**
```bash
#!/bin/bash
```
- First line of every script
- Tells system which interpreter to use
- Must be first line (no blank lines before it)

**Variables:**
```bash
#!/bin/bash
name="Richard"
age=42
echo "Name: $name, Age: $age"
```
- Store data for later use
- Reference with `$` prefix
- No spaces around `=` sign

**User Input:**
```bash
#!/bin/bash
echo "Enter your name:"
read username
echo "Hello, $username!"
```
- `read` command captures user input
- Stores input in variable

**Loops (For Loops):**
```bash
#!/bin/bash
for i in {1..10}; do
    echo "Number: $i"
done
```
- Iterate through sequences/ranges
- `{1..10}` creates range 1 to 10
- `do` starts loop body
- `done` ends loop

**Conditional Statements:**
```bash
#!/bin/bash
if [ "$name" = "Stewart" ]; then
    echo "Welcome Stewart! Secret: THM_Script"
else
    echo "Access denied!"
fi
```
- `if/then/else/fi` structure
- Square brackets `[ ]` for conditions
- `=` for string comparison
- `-eq` for number comparison
- Spaces inside brackets are REQUIRED
- `fi` ends conditional (if backwards)

**Comments:**
```bash
# Single line comment

<#
Multi-line
comment block
#>
```
- `#` for single-line comments
- Document your code
- Explain complex logic

**Script Execution:**
```bash
chmod +x script.sh    # Make executable
./script.sh           # Run script
```

### **Task 5: The Locker Script**

**Challenge:** Create authentication script combining loops, conditionals, and variables.

**Requirements:**
- Collect 3 inputs: username, company name, PIN
- Use for loop to iterate 3 times
- Use conditionals to prompt for each input
- Validate against expected values
- Display success or failure message

**Script Structure:**
```bash
#!/bin/bash

# Variables
username=""
companyname=""
pin=""

# Loop to collect 3 inputs
for i in {1..3}; do
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

# Authentication check
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

**Key Concepts:**
- Combining loops and conditionals
- Sequential input collection
- Multiple condition validation with `&&` (AND operator)
- String comparison in conditionals
- `-eq` for numeric comparison

**Question:**
- What should output be for successful authentication? → `Authentication Successful. You can now access your locker, John.`

### **Task 6: Practical Exercise**

**Challenge:** Modify existing script to search for keyword in log files.

**Given Information:**
- **Script:** `flag_hunt.sh`
- **Keyword/Flag:** `thm-flag01-script`
- **Directory:** `/var/log`
- **Hint:** Fill empty quotes `""` in 3 places (no spaces between quotes)

**Required Steps:**

**1. Gain root access:**
```bash
sudo su
# Password: user@Tryhackme
```

**2. Edit the script:**
```bash
nano flag_hunt.sh
```

**3. Fill in the 3 empty variables:**
```bash
#!/bin/bash

# Defining the directory to search our flag
directory="/var/log"                    # CHANGE 1

# Defining the flag to search
flag="thm-flag01-script"                # CHANGE 2

echo "Flag search in directory: $directory in progress..."

# Defining for loop to iterate over all files with .log extension
for file in "$directory"/*.log; do      # CHANGE 3
    # Check if the file contains the flag
    if grep -q "$flag" "$file"; then
        # Print the filename
        echo "Flag found in: $(basename "$file")"
    fi
done
```

**4. Make executable and run:**
```bash
chmod +x flag_hunt.sh
./flag_hunt.sh
```

**Output:**
```
Flag found in: authentication.log
```

**5. Find where cat is sleeping:**
```bash
cd /var/log
grep "cat" authentication.log
```

**Questions:**
1. Which file has the keyword? → `authentication.log`
2. Where is the cat sleeping? → `under the table`

**Key Learning Points:**
- Variable usage in scripts (`$directory`, `$flag`)
- For loops with wildcards (`*.log`)
- `grep -q` for silent searching (no output, just exit status)
- `basename` to extract filename from path
- Searching log files for keywords
- Real-world troubleshooting scenario

---

## Key Takeaways

### **Why Shells Matter:**

**1. Efficiency:**
- Commands execute faster than GUI clicks
- Automation saves time on repetitive tasks
- Remote management of servers (no GUI needed)

**2. Power:**
- Access to system internals
- Fine-grained control over operations
- Combine commands for complex operations

**3. Resource-Friendly:**
- Lower memory and CPU usage than GUI
- Essential for servers (often no GUI installed)
- Works over slow network connections

### **Bash vs Other Shells:**

**Use Bash when:**
- Working on Linux systems (most common default)
- Writing portable scripts
- Learning shell scripting basics
- Following online tutorials (most use Bash)

**Use Zsh when:**
- Want enhanced tab completion
- Need better customization (Oh My Zsh framework)
- Working on macOS (default since 2019)
- Want modern features and plugins

**Use Sh when:**
- Writing POSIX-compliant scripts
- Maximum portability across Unix systems
- System-level scripts (minimal dependencies)

### **Scripting Best Practices:**

1. **Always use shebang:** `#!/bin/bash`
2. **Comment your code:** Explain complex logic
3. **Use meaningful variable names:** `username` not `x`
4. **Check for errors:** Use conditionals to validate input
5. **Test incrementally:** Don't write entire script then test
6. **Use proper permissions:** `chmod +x` for execution
7. **Quote variables:** Use `"$variable"` not `$variable` (prevents issues with spaces)

---

## Practical Applications for System Administrators

### **Daily Operations:**

**1. System Monitoring:**
```bash
# Check disk space
df -h

# Check memory usage
free -h

# Check running processes
ps aux | grep processname
```

**2. Log Analysis:**
```bash
# Search for errors in system logs
grep "error" /var/log/syslog

# Count occurrences
grep -c "failed" /var/log/auth.log

# View recent log entries
tail -f /var/log/syslog
```

**3. User Management:**
```bash
# List users
cat /etc/passwd

# Check user's groups
groups username

# Find logged-in users
who
```

### **Automation Scenarios:**

**1. Backup Script:**
```bash
#!/bin/bash
backup_dir="/backup/$(date +%Y%m%d)"
mkdir -p $backup_dir
cp -r /important/data $backup_dir
echo "Backup completed: $backup_dir"
```

**2. Service Health Check:**
```bash
#!/bin/bash
services=("nginx" "mysql" "redis")
for service in "${services[@]}"; do
    if systemctl is-active --quiet $service; then
        echo "$service: Running"
    else
        echo "$service: STOPPED - ALERT!"
    fi
done
```

**3. Disk Space Alert:**
```bash
#!/bin/bash
threshold=90
usage=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ $usage -gt $threshold ]; then
    echo "WARNING: Disk usage at ${usage}%"
fi
```

### **Career Relevance:**

**For Junior Sysadmin Roles:**
- Shell proficiency is **mandatory** for Linux administration
- Automation capability = competitive advantage
- Scripting skills often tested in technical interviews

**For Security Roles:**
- Log analysis and investigation
- Incident response automation
- Security tool operation (many are CLI-based)

**For DevOps/Cloud:**
- Infrastructure automation
- CI/CD pipeline scripting
- Container and orchestration management

**Certifications Where Shell Skills Appear:**
- CompTIA Linux+
- RHCSA (Red Hat Certified System Administrator)
- LFCS (Linux Foundation Certified Sysadmin)
- Security+ (log analysis scenarios)

---

## Repository Structure

```
tryhackme-linux-shells/
├── README.md (this file)
├── notes.md (detailed explanations)
├── commands-cheatsheet.md (quick reference)
└── screenshots/
    ├── task-01-introduction-to-linux-shells.png
    ├── task-02-how-to-interact-with-a-shell.png
    ├── task-03-types-of-shells-part1.png
    ├── task-04-scripting-part1.png
    ├── task-05-the-locker-script.png
    └── task-06-practical-exercise.png
```

---

## Progression Path

**Foundation:** Linux Shells (this room)  
**Next Steps:**
- Bash Scripting (TryHackMe)
- Linux Fundamentals 1-3 (TryHackMe)
- Linux Privilege Escalation
- Advanced Bash scripting tutorials

**Real-World Practice:**
- Create automation scripts for daily tasks
- Build system monitoring tools
- Contribute to open-source shell scripts on GitHub
- Practice on personal Linux VM or Raspberry Pi

---

## Common Pitfalls & Solutions

**Problem 1: Script won't execute**
```bash
# Solution: Make it executable
chmod +x script.sh
```

**Problem 2: "Command not found" when running script**
```bash
# Wrong: script.sh
# Right: ./script.sh (must include ./ for current directory)
```

**Problem 3: Spaces in conditionals**
```bash
# Wrong: if [$x=5]; then
# Right: if [ "$x" = "5" ]; then
# Spaces are REQUIRED inside brackets
```

**Problem 4: Variable not expanding**
```bash
# Wrong: echo $PATH    (can break with special characters)
# Right: echo "$PATH"  (always quote variables)
```

**Problem 5: Can't find file in script**
```bash
# Use absolute paths in scripts
directory="/var/log"   # Not "var/log"
```

---

## Notes

- **Completion Date:** February 2026
- **Platform:** TryHackMe - Cyber Security 101 Path
- **Documentation:** Hands-on practical exercises with screenshots
- **AI Assistance:** Documentation created with Claude (Anthropic) - see commit messages for transparency

---

**Master Linux shells = Foundation for system administration, cybersecurity, and DevOps careers. Essential skill for any IT professional.**
