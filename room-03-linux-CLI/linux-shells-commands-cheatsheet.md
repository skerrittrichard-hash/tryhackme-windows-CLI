# Linux Shells Commands Cheatsheet

**TryHackMe - Linux Shells Room**  
**Quick Reference for System Administrators**

---

## Essential Shell Commands

### File System Navigation
| Command | Purpose | Example |
|---------|---------|---------|
| `ls` | List directory contents | `ls -la` |
| `cd` | Change directory | `cd /var/log` |
| `pwd` | Print working directory | `pwd` |
| `mkdir` | Create directory | `mkdir newfolder` |
| `rmdir` | Remove empty directory | `rmdir oldfolder` |
| `rm` | Remove files/directories | `rm file.txt` or `rm -r folder/` |
| `cp` | Copy files/directories | `cp file.txt backup.txt` |
| `mv` | Move/rename | `mv old.txt new.txt` |
| `touch` | Create empty file | `touch newfile.txt` |
| `find` | Search for files | `find / -name "*.log"` |

### File Viewing & Editing
| Command | Purpose | Example |
|---------|---------|---------|
| `cat` | Display file contents | `cat file.txt` |
| `less` | Page through file | `less /var/log/syslog` |
| `more` | Page through file (older) | `more file.txt` |
| `head` | Show first lines | `head -n 10 file.txt` |
| `tail` | Show last lines | `tail -f /var/log/syslog` |
| `nano` | Text editor (beginner) | `nano file.txt` |
| `vim` | Text editor (advanced) | `vim file.txt` |
| `grep` | Search text in files | `grep "error" file.txt` |

### File Information
| Command | Purpose | Example |
|---------|---------|---------|
| `file` | Determine file type | `file document.pdf` |
| `stat` | Detailed file info | `stat file.txt` |
| `wc` | Count lines/words/chars | `wc -l file.txt` |
| `du` | Disk usage | `du -sh folder/` |
| `df` | Disk space | `df -h` |

### Permissions & Ownership
| Command | Purpose | Example |
|---------|---------|---------|
| `chmod` | Change permissions | `chmod +x script.sh` |
| `chown` | Change owner | `chown user:group file.txt` |
| `chgrp` | Change group | `chgrp developers file.txt` |
| `umask` | Set default permissions | `umask 022` |

### System Information
| Command | Purpose | Example |
|---------|---------|---------|
| `whoami` | Current username | `whoami` |
| `hostname` | Computer name | `hostname` |
| `uname -a` | System information | `uname -a` |
| `uptime` | System uptime | `uptime` |
| `date` | Current date/time | `date` |
| `cal` | Calendar | `cal` |
| `w` | Who is logged in | `w` |
| `last` | Login history | `last` |

### Process Management
| Command | Purpose | Example |
|---------|---------|---------|
| `ps` | Show processes | `ps aux` |
| `top` | Interactive process viewer | `top` |
| `htop` | Enhanced top | `htop` |
| `kill` | Terminate process | `kill 1234` |
| `killall` | Kill by name | `killall firefox` |
| `pkill` | Kill by pattern | `pkill -u username` |
| `bg` | Background job | `bg %1` |
| `fg` | Foreground job | `fg %1` |
| `jobs` | List jobs | `jobs` |

### Network Commands
| Command | Purpose | Example |
|---------|---------|---------|
| `ping` | Test connectivity | `ping google.com` |
| `ifconfig` | Network config (older) | `ifconfig` |
| `ip` | Network config (modern) | `ip addr show` |
| `netstat` | Network statistics | `netstat -tulpn` |
| `ss` | Socket statistics | `ss -tulpn` |
| `curl` | Transfer data | `curl https://example.com` |
| `wget` | Download files | `wget https://example.com/file.txt` |
| `ssh` | Secure shell | `ssh user@server` |
| `scp` | Secure copy | `scp file.txt user@server:/path/` |

### Package Management (Debian/Ubuntu)
| Command | Purpose | Example |
|---------|---------|---------|
| `apt update` | Update package list | `sudo apt update` |
| `apt upgrade` | Upgrade packages | `sudo apt upgrade` |
| `apt install` | Install package | `sudo apt install nginx` |
| `apt remove` | Remove package | `sudo apt remove nginx` |
| `apt search` | Search packages | `apt search python` |

---

## Shell Identification

### Check Your Shell
| Command | Output | Purpose |
|---------|--------|---------|
| `echo $0` | `/bin/bash` | Current shell |
| `echo $SHELL` | `/bin/bash` | Default shell |
| `cat /etc/shells` | List of shells | Available shells |
| `bash --version` | Version info | Bash version |
| `zsh --version` | Version info | Zsh version |

### Switch Shells
```bash
bash      # Switch to Bash
zsh       # Switch to Zsh
sh        # Switch to Bourne shell
exit      # Return to previous shell
```

### Change Default Shell
```bash
chsh -s /bin/zsh      # Change to Zsh
chsh -s /bin/bash     # Change to Bash
# Logout/login for change to take effect
```

---

## ls Command Options

| Option | Purpose | Example |
|--------|---------|---------|
| `ls` | Basic listing | `ls` |
| `ls -l` | Long format (detailed) | `ls -l` |
| `ls -a` | Show hidden files | `ls -a` |
| `ls -la` | Long + hidden | `ls -la` |
| `ls -lh` | Human-readable sizes | `ls -lh` |
| `ls -lt` | Sort by time (newest first) | `ls -lt` |
| `ls -lS` | Sort by size (largest first) | `ls -lS` |
| `ls -R` | Recursive | `ls -R` |
| `ls -d */` | Directories only | `ls -d */` |
| `ls *.txt` | Filter by extension | `ls *.txt` |

---

## grep Command Options

| Option | Purpose | Example |
|--------|---------|---------|
| `grep "pattern" file` | Basic search | `grep "error" syslog` |
| `grep -i` | Case-insensitive | `grep -i "error" file.txt` |
| `grep -r` | Recursive search | `grep -r "TODO" /project/` |
| `grep -n` | Show line numbers | `grep -n "error" file.txt` |
| `grep -c` | Count matches | `grep -c "failed" auth.log` |
| `grep -v` | Invert (exclude pattern) | `grep -v "comment" file.txt` |
| `grep -l` | List filenames only | `grep -l "password" *.conf` |
| `grep -q` | Quiet (no output) | `grep -q "error" file.txt` |
| `grep -A 3` | 3 lines after match | `grep -A 3 "error" log` |
| `grep -B 3` | 3 lines before match | `grep -B 3 "error" log` |
| `grep -C 3` | 3 lines context | `grep -C 3 "error" log` |

---

## Bash Scripting Quick Reference

### Script Template
```bash
#!/bin/bash
# Script: name.sh
# Author: Your Name
# Date: YYYY-MM-DD
# Purpose: What this script does

# Variables
variable_name="value"

# Main logic
echo "Starting script..."
# Your commands here

# Completion
echo "Script completed"
exit 0
```

### Variables
```bash
# Create variable (no spaces around =)
name="Richard"
age=42

# Use variable (with $ prefix)
echo $name
echo "$name"          # Preferred (quoted)
echo "${name}"        # Best practice (curly braces)

# User input
read variable_name
echo "Enter name:"
read username
echo "Hello $username"

# Command substitution
today=$(date +%Y-%m-%d)
files=$(ls | wc -l)
```

### Special Variables
```bash
$0          # Script name
$1 $2 $3    # Arguments (first, second, third)
$#          # Number of arguments
$@          # All arguments
$?          # Exit status of last command
$$          # Process ID (PID)
$!          # PID of last background job
$HOME       # Home directory
$PWD        # Current directory
$USER       # Current username
```

### Conditionals
```bash
# If statement
if [ condition ]; then
    commands
fi

# If/else
if [ condition ]; then
    commands_if_true
else
    commands_if_false
fi

# If/elif/else
if [ condition1 ]; then
    commands1
elif [ condition2 ]; then
    commands2
else
    commands3
fi
```

### Comparison Operators

**String Comparison:**
```bash
[ "$a" = "$b" ]     # Equal
[ "$a" != "$b" ]    # Not equal
[ -z "$a" ]         # Empty string
[ -n "$a" ]         # Not empty string
```

**Numeric Comparison:**
```bash
[ $a -eq $b ]       # Equal
[ $a -ne $b ]       # Not equal
[ $a -gt $b ]       # Greater than
[ $a -ge $b ]       # Greater or equal
[ $a -lt $b ]       # Less than
[ $a -le $b ]       # Less or equal
```

**File Tests:**
```bash
[ -f file ]         # File exists
[ -d directory ]    # Directory exists
[ -r file ]         # Readable
[ -w file ]         # Writable
[ -x file ]         # Executable
[ -s file ]         # File not empty
[ file1 -nt file2 ] # file1 newer than file2
[ file1 -ot file2 ] # file1 older than file2
```

**Logical Operators:**
```bash
[ cond1 ] && [ cond2 ]    # AND
[ cond1 ] || [ cond2 ]    # OR
[ ! cond ]                # NOT
```

### Loops

**For Loop (Range):**
```bash
for i in {1..10}; do
    echo "Number: $i"
done
```

**For Loop (List):**
```bash
for item in apple banana cherry; do
    echo "Fruit: $item"
done
```

**For Loop (Files):**
```bash
for file in *.txt; do
    echo "Processing: $file"
done
```

**For Loop (Array):**
```bash
servers=("server1" "server2" "server3")
for server in "${servers[@]}"; do
    echo "Checking: $server"
done
```

**While Loop:**
```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

**Until Loop:**
```bash
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### Functions
```bash
# Define function
function_name() {
    echo "Inside function"
    # $1 = first argument to function
    # $2 = second argument
}

# Call function
function_name argument1 argument2
```

### Script Execution
```bash
# Make executable
chmod +x script.sh

# Run script
./script.sh
bash script.sh
/full/path/script.sh

# Run with arguments
./script.sh arg1 arg2

# Background execution
./script.sh &

# Redirect output
./script.sh > output.log 2>&1
```

---

## Common Scenarios: "How Do I...?"

### File Operations
| Scenario | Command |
|----------|---------|
| List all files including hidden? | `ls -la` |
| Find files by name? | `find /path -name "filename"` |
| Find files by extension? | `find /path -name "*.txt"` |
| Find files modified today? | `find /path -mtime 0` |
| Find large files (>100MB)? | `find /path -size +100M` |
| Copy directory recursively? | `cp -r source/ destination/` |
| Delete directory and contents? | `rm -rf directory/` |
| Create multiple directories? | `mkdir -p path/to/nested/dirs` |

### Text Processing
| Scenario | Command |
|----------|---------|
| Count lines in file? | `wc -l file.txt` |
| Count words in file? | `wc -w file.txt` |
| Show first 10 lines? | `head -n 10 file.txt` |
| Show last 10 lines? | `tail -n 10 file.txt` |
| Follow log file? | `tail -f /var/log/syslog` |
| Find text in files? | `grep -r "pattern" /path/` |
| Replace text in file? | `sed 's/old/new/g' file.txt` |
| Remove duplicate lines? | `sort file.txt \| uniq` |

### System Administration
| Scenario | Command |
|----------|---------|
| Check disk space? | `df -h` |
| Check memory usage? | `free -h` |
| Check CPU info? | `lscpu` or `cat /proc/cpuinfo` |
| Find process by name? | `ps aux \| grep processname` |
| Kill process by PID? | `kill 1234` |
| Kill process by name? | `killall processname` |
| Check running services? | `systemctl list-units --type=service` |
| Start/stop service? | `sudo systemctl start/stop servicename` |

### Permissions
| Scenario | Command |
|----------|---------|
| Make script executable? | `chmod +x script.sh` |
| Give read/write to all? | `chmod 666 file.txt` |
| Give all permissions? | `chmod 777 file.txt` |
| Recursive permissions? | `chmod -R 755 directory/` |
| Change owner? | `chown user:group file.txt` |
| Recursive ownership? | `chown -R user:group directory/` |

### Searching & Filtering
| Scenario | Command |
|----------|---------|
| Find in current dir? | `grep "pattern" *` |
| Case-insensitive search? | `grep -i "pattern" file.txt` |
| Search multiple files? | `grep "pattern" *.log` |
| Search recursively? | `grep -r "pattern" /path/` |
| Count occurrences? | `grep -c "pattern" file.txt` |
| Show line numbers? | `grep -n "pattern" file.txt` |
| Exclude pattern? | `grep -v "pattern" file.txt` |

### Archives & Compression
| Scenario | Command |
|----------|---------|
| Create tar archive? | `tar -cvf archive.tar files/` |
| Extract tar archive? | `tar -xvf archive.tar` |
| Create tar.gz? | `tar -czvf archive.tar.gz files/` |
| Extract tar.gz? | `tar -xzvf archive.tar.gz` |
| Create zip? | `zip -r archive.zip files/` |
| Extract zip? | `unzip archive.zip` |

---

## Useful Command Combinations (Pipes)

```bash
# Find top 10 largest files
find /path -type f -exec ls -lh {} \; | sort -k5 -hr | head -n 10

# Count files by extension
find . -type f | sed 's/.*\.//' | sort | uniq -c

# Find processes using most memory
ps aux | sort -k4 -nr | head -n 10

# Find processes using most CPU
ps aux | sort -k3 -nr | head -n 10

# Check failed login attempts
grep "Failed password" /var/log/auth.log | wc -l

# Find files containing text, show filenames only
grep -rl "TODO" /project/

# Monitor log file for errors in real-time
tail -f /var/log/syslog | grep "error"

# List unique IPs from access log
cat access.log | awk '{print $1}' | sort | uniq -c | sort -nr
```

---

## Quick Tips & Tricks

### Command History
```bash
history              # Show command history
!100                 # Execute command #100 from history
!!                   # Execute last command
!grep                # Execute last grep command
Ctrl+R               # Reverse search history
```

### Shortcuts
```bash
Ctrl+A               # Go to start of line
Ctrl+E               # Go to end of line
Ctrl+U               # Delete to start of line
Ctrl+K               # Delete to end of line
Ctrl+W               # Delete word before cursor
Ctrl+L               # Clear screen (same as 'clear')
Ctrl+C               # Cancel current command
Ctrl+D               # Exit shell (logout)
Ctrl+Z               # Suspend current process
Tab                  # Auto-complete
```

### Redirection
```bash
command > file       # Redirect output (overwrite)
command >> file      # Redirect output (append)
command 2> file      # Redirect errors
command &> file      # Redirect output and errors
command 2>&1         # Redirect errors to output
command < file       # Use file as input
command1 | command2  # Pipe output to another command
```

### Background Jobs
```bash
command &            # Run in background
jobs                 # List background jobs
fg %1                # Bring job 1 to foreground
bg %1                # Resume job 1 in background
Ctrl+Z               # Suspend current job
kill %1              # Kill job 1
```

### Aliases (add to ~/.bashrc)
```bash
alias ll='ls -la'
alias ..='cd ..'
alias grep='grep --color=auto'
alias update='sudo apt update && sudo apt upgrade'
```

---

## Script Debugging

```bash
# Run script with debug output
bash -x script.sh

# Add debug mode in script
#!/bin/bash
set -x               # Enable debugging
set +x               # Disable debugging

# Check syntax without running
bash -n script.sh

# Exit on error
set -e

# Exit on undefined variable
set -u
```

---

## Environment Variables

```bash
# View all environment variables
env
printenv

# Set variable (current session)
export VAR_NAME="value"

# Permanent variables (add to ~/.bashrc)
echo 'export VAR_NAME="value"' >> ~/.bashrc
source ~/.bashrc

# Common variables
$HOME                # Home directory
$USER                # Username
$PATH                # Executable search path
$PWD                 # Current directory
$SHELL               # Default shell
$HOSTNAME            # Computer name
```

---

## Permission Numbers

```bash
chmod 755 file       # rwxr-xr-x (common for scripts)
chmod 644 file       # rw-r--r-- (common for files)
chmod 600 file       # rw------- (private files)
chmod 777 file       # rwxrwxrwx (full access - avoid)
chmod 700 file       # rwx------ (owner only)
```

**Permission Calculation:**
- Read (r) = 4
- Write (w) = 2
- Execute (x) = 1

**Example: 755**
- Owner: 7 (4+2+1) = rwx
- Group: 5 (4+0+1) = r-x
- Others: 5 (4+0+1) = r-x

---

## Resources

**Learn More:**
- `man command` - Manual page for any command
- `command --help` - Quick help
- https://explainshell.com - Command explanation
- https://shellcheck.net - Script linting

**Practice:**
- TryHackMe Linux rooms
- OverTheWire Bandit challenges
- Personal Linux VM or Raspberry Pi

---

**Print this page and keep it at your desk for quick reference during system administration tasks!**
