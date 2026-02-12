# Linux Shells: Detailed Notes

**TryHackMe - Cyber Security 101 Path**  
**Room: Linux Shells**

---

## Task 1: Introduction to Linux Shells

### What is a Shell?

A **shell** is a program that acts as an interface between the user and the operating system. It interprets commands entered by the user and passes them to the OS kernel for execution.

**Think of it like this:**
- **You (User)** → Speak to the waiter (Shell) → Kitchen (Operating System) → Food arrives (Command executed)
- The shell translates your commands into instructions the OS can understand

### Two Types of Interfaces

**1. Graphical User Interface (GUI):**
- Click through menus and icons
- Visual, intuitive for beginners
- Examples: Windows Explorer, macOS Finder, GNOME, KDE
- Higher resource usage (memory, CPU)

**2. Command-Line Interface (CLI):**
- Type commands as text
- Steeper learning curve but more powerful
- Examples: Bash, PowerShell, Zsh
- Lower resource usage
- Faster for experienced users
- Essential for remote server management

### Why Learn Shells?

**For System Administrators:**
- Most servers run Linux without GUI (headless)
- Remote management via SSH requires shell knowledge
- Automation of repetitive tasks
- Faster than GUI for experienced users

**For Cybersecurity:**
- Log analysis and investigation
- Security tool operation (many CLI-based)
- Incident response automation
- Penetration testing tools use command line

**For DevOps/Cloud:**
- Infrastructure as Code (IaC)
- CI/CD pipeline scripting
- Container orchestration
- Cloud resource management (AWS CLI, Azure CLI)

---

## Task 2: How to Interact with a Shell?

### Identifying Your Shell

**Check current shell:**
```bash
echo $0
```
**Output:** `/bin/bash` (or current shell path)

**What this does:**
- `$0` is a special variable containing the name of the current shell
- `echo` displays the value

**Check default shell:**
```bash
echo $SHELL
```
**Output:** `/bin/bash` (your default login shell)

**Difference between `$0` and `$SHELL`:**
- `$0` = **current** shell you're using right now
- `$SHELL` = **default** shell assigned to your user account
- They differ if you switch shells temporarily

**Example:**
```bash
$ echo $SHELL
/bin/bash          # Your default shell

$ zsh              # Switch to zsh
% echo $0
zsh                # Current shell changed

% echo $SHELL
/bin/bash          # Default shell unchanged
```

### Essential Linux Commands

#### **ls - List Directory Contents**

**Basic usage:**
```bash
ls
```
**Output:** Lists files and directories in current location

**Common options:**

**`ls -l` (long format):**
```bash
$ ls -l
drwxr-xr-x 2 user user 4096 Feb 12 10:00 Documents
-rw-r--r-- 1 user user  156 Feb 12 09:30 file.txt
```
**Columns explained:**
- `drwxr-xr-x` = Permissions (d=directory, -=file)
- `2` = Number of links
- `user user` = Owner and group
- `4096` = Size in bytes
- `Feb 12 10:00` = Last modified date/time
- `Documents` = Name

**`ls -a` (all files, including hidden):**
```bash
$ ls -a
.
..
.bashrc
.hidden_file
Documents
file.txt
```
- Files starting with `.` are hidden
- `.` = current directory
- `..` = parent directory
- `.bashrc` = hidden configuration file

**`ls -la` (combine both options):**
```bash
$ ls -la
total 28
drwxr-xr-x 3 user user 4096 Feb 12 10:00 .
drwxr-xr-x 5 root root 4096 Feb 10 15:00 ..
-rw-r--r-- 1 user user  220 Feb 01 08:00 .bashrc
drwxr-xr-x 2 user user 4096 Feb 12 10:00 Documents
-rw-r--r-- 1 user user  156 Feb 12 09:30 file.txt
```
- Long format + hidden files
- Shows permissions, ownership, and timestamps for everything

**Other useful options:**
```bash
ls -lh        # Human-readable sizes (KB, MB, GB)
ls -lt        # Sort by modification time (newest first)
ls -lS        # Sort by size (largest first)
ls -R         # Recursive (show subdirectories)
```

#### **cd - Change Directory**

```bash
cd /path/to/directory      # Absolute path
cd Documents               # Relative path
cd ..                      # Parent directory
cd ~                       # Home directory
cd -                       # Previous directory
cd                         # Home directory (shortcut)
```

**Examples:**
```bash
$ pwd
/home/user

$ cd /var/log
$ pwd
/var/log

$ cd ..
$ pwd
/var

$ cd ~
$ pwd
/home/user
```

**Path types:**
- **Absolute path:** Starts with `/` (e.g., `/var/log/syslog`)
- **Relative path:** Relative to current location (e.g., `Documents/file.txt`)

#### **cat - Display File Contents**

```bash
cat filename.txt
```
**Displays entire file contents to terminal**

**Examples:**
```bash
# View single file
cat /etc/passwd

# View multiple files
cat file1.txt file2.txt

# Number lines
cat -n file.txt

# Show non-printing characters
cat -A file.txt
```

**When to use cat:**
- Small text files you want to view completely
- Combining multiple files
- Quick content checks

**When NOT to use cat:**
- Large files (use `less` or `more` instead)
- Binary files (will display garbage)

#### **grep - Search for Patterns**

```bash
grep "pattern" filename
```
**Searches for text patterns inside files**

**Basic examples:**
```bash
# Find lines containing "error"
grep "error" /var/log/syslog

# Case-insensitive search
grep -i "error" file.txt

# Count matches
grep -c "failed" auth.log

# Show line numbers
grep -n "password" config.txt

# Recursive search in directory
grep -r "TODO" /home/user/projects/

# Invert match (lines NOT containing pattern)
grep -v "comment" file.txt
```

**Real-world examples:**
```bash
# Find failed login attempts
grep "Failed password" /var/log/auth.log

# Find your IP address
ip addr | grep "inet "

# Find running processes
ps aux | grep "nginx"

# Search multiple files
grep "config" *.conf
```

**Common grep options:**
- `-i` = Case-insensitive
- `-c` = Count matches
- `-n` = Show line numbers
- `-v` = Invert match (exclude pattern)
- `-r` = Recursive search
- `-l` = List filenames only (not content)
- `-q` = Quiet mode (no output, exit status only)

---

## Task 3: Types of Shells

### Viewing Available Shells

**List all installed shells:**
```bash
cat /etc/shells
```

**Typical output:**
```bash
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh
```

### Shell Comparison

#### **Bash (Bourne Again Shell)**

**Most popular Linux shell - default on most distributions**

**History:**
- Released 1989 by Brian Fox (GNU Project)
- Enhanced replacement for original Bourne shell (sh)
- "Again" = pun on "born again"

**Key Features:**
- Command history (up/down arrows)
- Tab completion
- Job control (background processes)
- Command substitution
- Shell scripting with functions
- Aliases and variables
- Arithmetic operations

**Strengths:**
- Ubiquitous (everywhere)
- Extensive documentation
- Most online tutorials use Bash
- POSIX compatible (mostly)
- Stable and reliable

**Weaknesses:**
- Less modern features than Zsh
- Basic tab completion
- Limited customization without plugins

**When to use Bash:**
- Default choice for most situations
- Writing portable scripts
- Following tutorials (most use Bash)
- Enterprise environments (widest compatibility)

#### **Zsh (Z Shell)**

**Modern, feature-rich alternative to Bash**

**History:**
- Created 1990 by Paul Falstad
- Named after Yale professor Zhong Shao
- Default shell on macOS since Catalina (2019)

**Key Features:**
- **Better tab completion:**
  - Context-aware completions
  - Correction suggestions
  - Completes partial matches
  
- **Enhanced customization:**
  - Oh My Zsh framework (themes & plugins)
  - Custom prompts with colors
  - Plugin ecosystem
  
- **Advanced features:**
  - Spelling correction
  - Global aliases
  - Better globbing (file matching)
  - Shared command history

**Example - Tab Completion Difference:**
```bash
# Bash: Basic completion
$ cd Do<TAB>
Documents/ Downloads/

# Zsh: Smart completion with menu
$ cd Do<TAB>
Documents/  Downloads/
(use arrows to select)
```

**Strengths:**
- Modern, powerful features
- Excellent customization
- Active development
- Great community (Oh My Zsh)

**Weaknesses:**
- Not installed by default on most Linux
- Slight learning curve for advanced features
- Scripts may need Bash compatibility mode

**When to use Zsh:**
- Personal workstation (interactive use)
- macOS systems
- Want advanced features and customization
- Developer-friendly environment

#### **Sh (Bourne Shell)**

**Original Unix shell - simple and portable**

**History:**
- Created 1979 by Stephen Bourne (Bell Labs)
- Original Unix shell
- Base for many modern shells

**Key Features:**
- POSIX compliant (standard)
- Minimal features
- Very portable
- Small memory footprint

**Strengths:**
- Maximum portability
- Available on all Unix systems
- Fast execution
- Simple, predictable behavior

**Weaknesses:**
- Lacks modern conveniences
- No command history
- Limited tab completion
- Basic scripting features

**When to use Sh:**
- System scripts requiring portability
- Embedded systems
- When size matters
- POSIX compliance required

### Switching Between Shells

**Temporary switch:**
```bash
$ bash              # Your current shell
$ zsh               # Switch to zsh
% echo $0
zsh
% exit              # Return to bash
```

**Change default shell permanently:**
```bash
chsh -s /bin/zsh
# Logout and login for change to take effect
```

**Run single command in different shell:**
```bash
zsh -c "echo Hello from zsh"
bash -c "source script.sh"
```

---

## Task 4: Scripting

### Script Structure

Every bash script follows this basic structure:

```bash
#!/bin/bash
# Comments explaining what the script does

# Variable definitions
variable_name="value"

# Commands and logic
command1
command2

# Output
echo "Script completed"
```

### The Shebang Line

```bash
#!/bin/bash
```

**What it does:**
- Tells system which interpreter to use
- Must be **first line** of script (no blank lines before)
- `#!` = "shebang" or "hashbang"
- `/bin/bash` = path to bash interpreter

**Other shebangs:**
```bash
#!/bin/bash          # Bash script
#!/bin/sh            # Bourne shell script
#!/usr/bin/env bash  # Portable bash (searches PATH)
#!/usr/bin/python3   # Python script
#!/usr/bin/env node  # Node.js script
```

### Variables

**Creating variables:**
```bash
name="Richard"
age=42
city="Hull"
```

**Rules:**
- No spaces around `=` sign
- Variable names: letters, numbers, underscores (can't start with number)
- Case-sensitive (`Name` ≠ `name`)
- No special characters in names

**Using variables:**
```bash
echo $name           # Output: Richard
echo "Name: $name"   # Output: Name: Richard
echo "Age: ${age}"   # Output: Age: 42 (curly braces optional but good practice)
```

**User input:**
```bash
#!/bin/bash
echo "Enter your name:"
read username
echo "Hello, $username!"
```

**Example execution:**
```
$ ./script.sh
Enter your name:
Richard
Hello, Richard!
```

**Special variables:**
```bash
$0       # Script name
$1       # First argument
$2       # Second argument
$#       # Number of arguments
$@       # All arguments
$?       # Exit status of last command
$$       # Current process ID
$HOME    # User's home directory
$PWD     # Current directory
```

### Loops

**For loop syntax:**
```bash
for variable in list; do
    commands
done
```

**Range examples:**
```bash
# Numbers 1 to 10
for i in {1..10}; do
    echo "Number: $i"
done

# Numbers 1 to 100 by 10
for i in {1..100..10}; do
    echo $i
done
# Output: 1 11 21 31 41 51 61 71 81 91

# Specific values
for color in red green blue; do
    echo "Color: $color"
done

# Files in directory
for file in *.txt; do
    echo "Processing: $file"
done
```

**C-style for loop:**
```bash
for ((i=1; i<=10; i++)); do
    echo "Count: $i"
done
```

**While loop:**
```bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

**Practical loop examples:**

**1. Backup multiple files:**
```bash
#!/bin/bash
for file in *.conf; do
    cp "$file" "$file.backup"
    echo "Backed up: $file"
done
```

**2. Check multiple servers:**
```bash
#!/bin/bash
servers=("server1" "server2" "server3")
for server in "${servers[@]}"; do
    ping -c 1 "$server" > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        echo "$server: UP"
    else
        echo "$server: DOWN"
    fi
done
```

### Conditional Statements

**If/then/else syntax:**
```bash
if [ condition ]; then
    commands_if_true
else
    commands_if_false
fi
```

**Important spacing rules:**
- Spaces required inside brackets: `[ condition ]` not `[condition]`
- Space after `[` and before `]`
- No space around `=` in assignments
- Space around `=` in comparisons

**String comparisons:**
```bash
if [ "$name" = "Stewart" ]; then
    echo "Welcome Stewart"
fi

if [ "$name" != "Stewart" ]; then
    echo "Not Stewart"
fi

# Check if variable is empty
if [ -z "$variable" ]; then
    echo "Variable is empty"
fi

# Check if variable is NOT empty
if [ -n "$variable" ]; then
    echo "Variable has value"
fi
```

**Numeric comparisons:**
```bash
if [ $age -eq 18 ]; then      # Equal
if [ $age -ne 18 ]; then      # Not equal
if [ $age -gt 18 ]; then      # Greater than
if [ $age -ge 18 ]; then      # Greater or equal
if [ $age -lt 18 ]; then      # Less than
if [ $age -le 18 ]; then      # Less or equal
```

**File checks:**
```bash
if [ -f file.txt ]; then      # File exists
if [ -d directory ]; then     # Directory exists
if [ -r file.txt ]; then      # File is readable
if [ -w file.txt ]; then      # File is writable
if [ -x script.sh ]; then     # File is executable
```

**Logical operators:**
```bash
# AND (both conditions must be true)
if [ "$name" = "John" ] && [ "$age" -gt 18 ]; then
    echo "John and over 18"
fi

# OR (either condition can be true)
if [ "$day" = "Saturday" ] || [ "$day" = "Sunday" ]; then
    echo "Weekend"
fi

# NOT
if [ ! -f file.txt ]; then
    echo "File does not exist"
fi
```

**If/elif/else (multiple conditions):**
```bash
#!/bin/bash
echo "Enter a number (1-3):"
read number

if [ $number -eq 1 ]; then
    echo "You chose one"
elif [ $number -eq 2 ]; then
    echo "You chose two"
elif [ $number -eq 3 ]; then
    echo "You chose three"
else
    echo "Invalid choice"
fi
```

**Nested conditionals:**
```bash
#!/bin/bash
if [ "$username" = "admin" ]; then
    echo "Admin user detected"
    if [ "$pin" = "1234" ]; then
        echo "Authentication successful"
    else
        echo "Wrong PIN"
    fi
else
    echo "Regular user"
fi
```

### Comments

**Single-line comments:**
```bash
# This is a comment
echo "Hello"  # This is also a comment
```

**Multi-line comment block:**
```bash
: '
This is a multi-line comment.
Everything here is ignored.
Useful for documentation.
'
```

**Best practices:**
```bash
#!/bin/bash
# Script: backup.sh
# Author: Richard
# Date: 2026-02-12
# Purpose: Backup important files
# Usage: ./backup.sh

# Variable definitions
backup_dir="/backup"  # Destination directory
date=$(date +%Y%m%d)  # Today's date in YYYYMMDD format

# Main backup logic
echo "Starting backup to $backup_dir/$date"
# ... rest of script
```

### Script Execution

**Make script executable:**
```bash
chmod +x script.sh
```

**Run script:**
```bash
./script.sh          # From current directory
bash script.sh       # Explicitly use bash
/full/path/script.sh # Absolute path
```

**Why `./` is required:**
- Linux doesn't execute scripts from current directory by default (security)
- `./` explicitly says "execute from current directory"
- Alternative: add directory to PATH (not recommended for personal scripts)

---

## Task 5: The Locker Script

### Challenge Overview

Create an authentication script that:
1. Collects 3 pieces of information (username, company, PIN)
2. Uses a for loop to iterate 3 times
3. Uses conditionals to prompt for correct input
4. Validates all inputs and grants/denies access

### Solution Breakdown

**Full script:**
```bash
#!/bin/bash

# Defining the variables
username=""
companyname=""
pin=""

# Defining the loop - iterate 3 times for 3 inputs
for i in {1..3}; do
    # Defining the conditional statements
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

# Checking if the user entered the correct details
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

### How It Works

**1. Variable initialization:**
```bash
username=""
companyname=""
pin=""
```
- Empty variables to store user input
- Initialized at script start

**2. For loop structure:**
```bash
for i in {1..3}; do
```
- Loop counter `i` goes from 1 to 3
- Each iteration collects one piece of information

**3. Conditional prompts:**
```bash
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
```
- First iteration (i=1): Ask for username
- Second iteration (i=2): Ask for company name
- Third iteration (i=3): Ask for PIN
- `read` command stores input in respective variable

**4. Authentication logic:**
```bash
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
```
- `&&` = AND operator (all conditions must be true)
- Checks all three inputs match expected values
- Only succeeds if ALL are correct

**5. Output:**
- Success: Grants access with personalized message
- Failure: Denies access

### Execution Example

**Successful authentication:**
```
$ ./locker.sh
Enter your Username:
John
Enter your Company name:
Tryhackme
Enter your PIN:
7385
Authentication Successful. You can now access your locker, John.
```

**Failed authentication:**
```
$ ./locker.sh
Enter your Username:
John
Enter your Company name:
Wrong
Enter your PIN:
7385
Authentication Denied!!
```

### Key Learning Points

**1. Sequential data collection:**
- Use loops to avoid repeating code
- Each iteration handles one input

**2. Conditional branching:**
- `if/elif/else` for multiple choices
- Numeric comparison with `-eq`

**3. Multiple condition validation:**
- `&&` ensures all conditions must be true
- Single failure = entire check fails

**4. User experience:**
- Clear prompts guide user
- Personalized success message
- Generic failure (don't reveal which input was wrong - security)

---

## Task 6: Practical Exercise

### The Challenge

**Given:** A script `flag_hunt.sh` with empty variables  
**Goal:** Fill in variables to search for keyword in log files  
**Keyword:** `thm-flag01-script`  
**Directory:** `/var/log`

### Common Confusion

**What many people get wrong:**
- Think "thm-flag01-script" is a **file name** to find
- Try commands like: `find / -name thm-flag01-script`

**What it actually is:**
- "thm-flag01-script" is a **keyword inside a file**
- It's the search term, not a filename

### Solution Steps

**1. Gain root privileges:**
```bash
sudo su
# Password: user@Tryhackme
```
- Need root to read system log files
- `/var/log` permissions restrict normal users

**2. View and edit the script:**
```bash
ls                    # Verify flag_hunt.sh exists
nano flag_hunt.sh     # Edit the script
```

**3. Original script with empty variables:**
```bash
#!/bin/bash

# Defining the directory to search our flag
directory=""

# Defining the flag to search
flag=""

echo "Flag search in directory: $directory in progress..."

# Defining for loop to iterate over all files with .log extension
for file in " "/*.log; do
    # Check if the file contains the flag
    if grep -q "$flag" "$file"; then
        # Print the filename
        echo "Flag found in: $(basename "$file")"
    fi
done
```

**4. Fill in the 3 empty spots:**
```bash
#!/bin/bash

# Defining the directory to search our flag
directory="/var/log"                          # CHANGE 1

# Defining the flag to search
flag="thm-flag01-script"                      # CHANGE 2

echo "Flag search in directory: $directory in progress..."

# Defining for loop to iterate over all files with .log extension
for file in "$directory"/*.log; do            # CHANGE 3
    # Check if the file contains the flag
    if grep -q "$flag" "$file"; then
        # Print the filename
        echo "Flag found in: $(basename "$file")"
    fi
done
```

**Critical details:**
- **Change 1:** Full path `/var/log/` (absolute path with leading `/`)
- **Change 2:** Exact keyword `thm-flag01-script` (NO SPACES inside quotes)
- **Change 3:** Insert `$directory` variable in the for loop

**5. Save and exit nano:**
- `Ctrl+O` → Save (Write Out)
- `Enter` → Confirm filename
- `Ctrl+X` → Exit

**6. Make executable and run:**
```bash
chmod +x flag_hunt.sh
./flag_hunt.sh
```

**Expected output:**
```
Flag search in directory: /var/log in progress...
Flag found in: authentication.log
```

### Understanding the Script Logic

**1. Variables:**
```bash
directory="/var/log"
flag="thm-flag01-script"
```
- Store paths and search terms
- Reusable throughout script

**2. For loop:**
```bash
for file in "$directory"/*.log; do
```
- `"$directory"/*.log` expands to `/var/log/*.log`
- `*.log` = wildcard matching all files ending in `.log`
- Loop iterates over each .log file in directory

**3. grep command:**
```bash
if grep -q "$flag" "$file"; then
```
- `grep` searches for text patterns
- `-q` = quiet mode (no output, just exit status)
- `"$flag"` = pattern to search for
- `"$file"` = file to search in
- Returns 0 (true) if found, 1 (false) if not

**4. basename command:**
```bash
echo "Flag found in: $(basename "$file")"
```
- `$(...)` = command substitution (run command, insert output)
- `basename /var/log/authentication.log` returns `authentication.log`
- Extracts filename from full path

### Finding Where the Cat is Sleeping

**Method 1: grep from /var/log:**
```bash
cd /var/log
grep "cat" authentication.log
```

**Method 2: grep with full path:**
```bash
grep "cat" /var/log/authentication.log
```

**Output (will show lines containing "cat"):**
```
The cat is sleeping under the table
```

**Answer:** `under the table`

### Key Takeaways

**1. Variable naming matters:**
- "Flag" in CTF context = keyword/target, not file
- Read instructions carefully

**2. Absolute vs relative paths:**
- `/var/log` = absolute (starts from root)
- `var/log` = relative (broken - won't work)

**3. Script debugging:**
- Test with `echo` statements to verify variables
- Check paths exist before running

**4. grep is powerful:**
- `-q` for scripting (exit status only)
- `-i` for case-insensitive
- `-n` for line numbers
- Combine with loops for bulk searching

**5. Command substitution:**
- `$(command)` runs command and inserts output
- Useful for dynamic values in scripts

---

## Summary: Essential Bash Scripting Concepts

### Must-Know Elements

**1. Shebang:** `#!/bin/bash` (first line always)

**2. Variables:**
```bash
name="value"      # No spaces around =
echo "$name"      # Use with $ prefix
```

**3. User Input:**
```bash
read variable_name
```

**4. Loops:**
```bash
for i in {1..10}; do
    commands
done
```

**5. Conditionals:**
```bash
if [ condition ]; then
    commands
elif [ condition2 ]; then
    commands
else
    commands
fi
```

**6. Comments:** `# Single line`

**7. Execution:**
```bash
chmod +x script.sh
./script.sh
```

### Common Pitfalls

**1. Forgetting shebang:**
- Script may execute with wrong interpreter

**2. Spaces in conditionals:**
```bash
# Wrong: if [$x=5]; then
# Right: if [ "$x" = "5" ]; then
```

**3. Not quoting variables:**
```bash
# Wrong: if [ $name = "John" ]; then
# Right: if [ "$name" = "John" ]; then
```

**4. Forgetting chmod +x:**
- Script won't be executable

**5. Using wrong path:**
```bash
# Wrong: script.sh
# Right: ./script.sh
```

### Best Practices

**1. Always quote variables:** `"$variable"`

**2. Use meaningful names:** `username` not `x`

**3. Comment your code:** Explain complex logic

**4. Test incrementally:** Don't write everything then test

**5. Check exit status:** `if [ $? -eq 0 ]`

**6. Use shellcheck:** Lint your scripts (shellcheck.net)

---

**Master these fundamentals = foundation for system automation, security scripting, and DevOps workflows.**
