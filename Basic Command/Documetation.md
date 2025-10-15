# Essential Linux Commands for DevOps & System Engineers

## Navigation & File System Basics

### `pwd` - Print Working Directory
Shows your current location in the file system.
```bash
pwd
# Output: /home/username/projects
```

### `cd` - Change Directory
Navigate between directories.
```bash
cd /var/log          # Go to absolute path
cd ..                # Go up one level
cd ~                 # Go to home directory
cd -                 # Go to previous directory
```

### `ls` - List Directory Contents
Display files and directories.
```bash
ls                   # Basic listing
ls -l                # Long format (permissions, size, date)
ls -la               # Include hidden files
ls -lh               # Human-readable sizes
ls -lt               # Sort by modification time
ls -ltr              # Reverse time sort (oldest first)
```

## Viewing & Reading Files

### `cat` - Concatenate and Display Files
Display entire file contents.
```bash
cat file.txt         # Display file
cat file1 file2      # Display multiple files
cat -n file.txt      # Show line numbers
```

### `less` - View Files Page by Page
Interactive file viewer (better than `more`).
```bash
less /var/log/syslog
# Navigate: Space (next page), b (previous page), / (search), q (quit)
```

### `head` - View Beginning of Files
Display first lines of a file.
```bash
head file.txt        # First 10 lines (default)
head -n 20 file.txt  # First 20 lines
head -n 5 *.log      # First 5 lines of all log files
```

### `tail` - View End of Files
Display last lines of a file.
```bash
tail file.txt        # Last 10 lines
tail -n 50 file.txt  # Last 50 lines
tail -f /var/log/syslog  # Follow file in real-time (CRITICAL for DevOps!)
tail -f app.log | grep ERROR  # Follow and filter
```

## Text Processing & Filtering

### `grep` - Search Text Patterns ⭐ ESSENTIAL
Search for patterns in files.
```bash
grep "error" app.log                    # Find "error" in file
grep -i "error" app.log                 # Case-insensitive
grep -r "TODO" /project                 # Recursive search
grep -v "debug" app.log                 # Invert match (exclude)
grep -c "error" app.log                 # Count matches
grep -n "error" app.log                 # Show line numbers
grep -E "error|warning" app.log         # Extended regex (OR)
grep -A 5 -B 5 "error" app.log         # Show 5 lines After and Before
```

### `sort` - Sort Lines
Sort file contents.
```bash
sort file.txt        # Alphabetical sort
sort -r file.txt     # Reverse sort
sort -n file.txt     # Numeric sort
sort -u file.txt     # Sort and remove duplicates
sort -k 2 file.txt   # Sort by 2nd column
```

### `uniq` - Remove Duplicate Lines
Filter adjacent duplicate lines (use with `sort`).
```bash
sort file.txt | uniq              # Remove duplicates
sort file.txt | uniq -c           # Count occurrences
sort file.txt | uniq -d           # Show only duplicates
```

### `wc` - Word Count
Count lines, words, and characters.
```bash
wc file.txt          # Lines, words, bytes
wc -l file.txt       # Count lines only
wc -w file.txt       # Count words
ls -l | wc -l        # Count files in directory
```

### `cut` - Extract Columns ⭐ ADDED
Extract specific fields from text.
```bash
cut -d: -f1 /etc/passwd          # Extract 1st field (delimiter :)
cut -d, -f1,3 data.csv           # Extract columns 1 and 3
ps aux | cut -c 1-50             # Extract first 50 characters
```

### `awk` - Pattern Scanning & Processing ⭐ ADDED
Powerful text processing tool.
```bash
awk '{print $1}' file.txt                    # Print first column
awk -F: '{print $1}' /etc/passwd            # Custom delimiter
awk '$3 > 50' data.txt                       # Filter rows where col3 > 50
awk '{sum+=$1} END {print sum}' numbers.txt  # Sum first column
df -h | awk 'NR>1 {print $5, $6}'           # Skip header, print usage
```

### `sed` - Stream Editor ⭐ ADDED
Edit text streams.
```bash
sed 's/old/new/' file.txt             # Replace first occurrence per line
sed 's/old/new/g' file.txt            # Replace all occurrences
sed -i 's/old/new/g' file.txt         # Edit file in-place
sed -n '10,20p' file.txt              # Print lines 10-20
sed '/pattern/d' file.txt             # Delete lines matching pattern
```

## I/O Redirections & Pipes

### Output Redirection
```bash
command > file.txt       # Redirect stdout to file (overwrite)
command >> file.txt      # Redirect stdout to file (append)
command 2> error.log     # Redirect stderr to file
command &> output.log    # Redirect both stdout and stderr
command > /dev/null 2>&1 # Discard all output
```

### Pipe `|` - Chain Commands
Send output of one command to another.
```bash
cat file.txt | grep "error" | wc -l           # Count error lines
ls -l | sort -k 5 -n | tail -10               # 10 largest files
ps aux | grep nginx | grep -v grep            # Find nginx processes
history | tail -100 | grep ssh                # Recent ssh commands
```

### `tee` - Read from stdin and Write to stdout and Files
View and save output simultaneously.
```bash
ls -l | tee output.txt                    # Display and save
command | tee -a log.txt                  # Append to file
echo "test" | tee file1.txt file2.txt     # Write to multiple files
```

## System Information

### `uptime` - System Uptime and Load
Show how long system has been running.
```bash
uptime
# Output: 10:30:45 up 15 days, 3:45, 2 users, load average: 0.50, 0.40, 0.35
```

### `env` - Environment Variables
Display or set environment variables.
```bash
env                  # Show all variables
env | grep PATH      # Show PATH variable
export VAR=value     # Set variable
```

### `which` - Locate Command Binary
Find the path of a command.
```bash
which python         # Output: /usr/bin/python
which -a python      # Show all instances in PATH
```

### `top` / `htop` - Process Monitoring ⭐ ADDED
Real-time system monitoring.
```bash
top                  # Interactive process viewer
htop                 # Better version (if installed)
# Keys: q (quit), k (kill), M (sort by memory), P (sort by CPU)
```

### `ps` - Process Status ⭐ ADDED
Snapshot of current processes.
```bash
ps aux               # All processes with details
ps aux | grep nginx  # Find specific process
ps -ef               # Full format listing
ps -u username       # Processes for specific user
```

### `df` - Disk Space ⭐ ADDED
Show disk usage.
```bash
df -h                # Human-readable format
df -i                # Inode usage (important for DevOps!)
df -T                # Show filesystem type
```

### `du` - Disk Usage ⭐ ADDED
Estimate file/directory space usage.
```bash
du -sh /var/log      # Summary, human-readable
du -h --max-depth=1  # Size of subdirectories
du -ah | sort -rh | head -20  # Top 20 largest files/dirs
```

### `free` - Memory Usage ⭐ ADDED
Display memory information.
```bash
free -h              # Human-readable
free -m              # In megabytes
```

## File Search

### `find` - Search for Files
Powerful file search tool.
```bash
find /path -name "*.log"                    # Find by name
find . -type f -name "*.txt"                # Files only
find . -type d -name "config"               # Directories only
find . -mtime -7                             # Modified in last 7 days
find . -size +100M                           # Files larger than 100MB
find . -name "*.log" -exec rm {} \;         # Find and delete
find . -name "*.tmp" -delete                # Delete found files
find /var/log -name "*.log" -mtime +30 -exec gzip {} \;  # Archive old logs
```

### `locate` - Fast File Search
Quick file search using database.
```bash
locate nginx.conf    # Fast search
locate -i FILE       # Case-insensitive
sudo updatedb        # Update locate database
```

## Help & Documentation

### `man` - Manual Pages
Read command documentation.
```bash
man ls               # Read ls manual
man -k network       # Search manuals for keyword
man 5 passwd         # Specific section (5 = file formats)
```

### `echo` - Display Text
Print text or variables.
```bash
echo "Hello World"
echo $PATH           # Display variable
echo -e "Line1\nLine2"  # Enable escape sequences
echo $?              # Last command exit status (0=success)
```

### `history` - Command History
View previous commands.
```bash
history              # Show command history
history | tail -20   # Last 20 commands
!100                 # Execute command #100
!!                   # Execute last command
!$                   # Last argument of previous command
Ctrl+R               # Reverse search history (interactive)
```

### `clear` - Clear Terminal Screen
```bash
clear                # Clear screen
Ctrl+L               # Keyboard shortcut to clear
```

## Aliases & Customization

### `alias` - Create Command Shortcuts
Create custom shortcuts for commands.
```bash
alias ll='ls -lah'              # Create alias
alias update='sudo apt update && sudo apt upgrade'
alias logs='tail -f /var/log/syslog'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'

# Make permanent: Add to ~/.bashrc or ~/.bash_aliases
unalias ll                       # Remove alias
alias                            # List all aliases
```

## Additional DevOps Essential Commands

### `chmod` - Change Permissions ⭐ CRITICAL
Modify file permissions.
```bash
chmod 755 script.sh              # rwxr-xr-x
chmod +x script.sh               # Add execute permission
chmod -R 644 /var/www            # Recursive
chmod u+w,g-w,o-w file           # User add write, group/other remove
```

### `chown` - Change Ownership ⭐ CRITICAL
Change file owner and group.
```bash
sudo chown user:group file.txt
sudo chown -R www-data:www-data /var/www
sudo chown user file.txt         # Change owner only
```

### `ln` - Create Links ⭐ ADDED
Create symbolic and hard links.
```bash
ln -s /path/to/file linkname     # Symbolic link (shortcut)
ln file hardlink                 # Hard link
ls -l                            # Links show with 'l' and '->'
```

### `tar` - Archive Files ⭐ CRITICAL
Compress and extract archives.
```bash
tar -czf archive.tar.gz /path    # Create compressed archive
tar -xzf archive.tar.gz          # Extract archive
tar -tzf archive.tar.gz          # List contents
tar -xzf archive.tar.gz -C /dest # Extract to specific location
```

### `wget` / `curl` - Download Files ⭐ CRITICAL
Download files from internet.
```bash
wget https://example.com/file.zip
wget -c url                      # Resume download
curl -O https://example.com/file # Download file
curl -I https://example.com      # Get headers only
curl -X POST -d "data" url       # POST request
```

### `systemctl` - Service Management ⭐ CRITICAL
Manage system services (systemd).
```bash
sudo systemctl start nginx       # Start service
sudo systemctl stop nginx        # Stop service
sudo systemctl restart nginx     # Restart service
sudo systemctl status nginx      # Check status
sudo systemctl enable nginx      # Enable at boot
sudo systemctl disable nginx     # Disable at boot
systemctl list-units --type=service  # List all services
```

### `journalctl` - View System Logs ⭐ CRITICAL
Query systemd journal.
```bash
journalctl -u nginx              # Logs for specific service
journalctl -f                    # Follow logs
journalctl -p err                # Only errors
journalctl --since "1 hour ago"  # Recent logs
journalctl -u nginx -n 50        # Last 50 lines
```

### `ssh` - Secure Shell ⭐ CRITICAL
Connect to remote servers.
```bash
ssh user@hostname
ssh -i key.pem user@hostname     # Use specific key
ssh -p 2222 user@hostname        # Custom port
ssh user@host 'command'          # Execute remote command
```

### `scp` - Secure Copy ⭐ CRITICAL
Copy files between systems.
```bash
scp file.txt user@host:/path     # Copy to remote
scp user@host:/path/file.txt .   # Copy from remote
scp -r folder user@host:/path    # Copy directory
```

### `rsync` - Sync Files ⭐ CRITICAL
Efficient file synchronization.
```bash
rsync -avz /source/ /dest/               # Local sync
rsync -avz /source/ user@host:/dest/     # Remote sync
rsync -avz --delete /source/ /dest/      # Mirror (delete extra files)
```

### `netstat` / `ss` - Network Statistics ⭐ ADDED
View network connections.
```bash
netstat -tuln                    # Listening ports
ss -tuln                         # Modern alternative
ss -tunap | grep :80             # What's using port 80
```

### `kill` / `pkill` - Terminate Processes ⭐ ADDED
Stop running processes.
```bash
kill PID                         # Terminate process
kill -9 PID                      # Force kill
pkill nginx                      # Kill by name
killall nginx                    # Kill all instances
```

## Pro Tips for DevOps

### Command Chaining
```bash
command1 && command2             # Run command2 if command1 succeeds
command1 || command2             # Run command2 if command1 fails
command1; command2               # Run both regardless
```

### Background Jobs
```bash
command &                        # Run in background
jobs                             # List background jobs
fg %1                            # Bring job 1 to foreground
bg %1                            # Resume job 1 in background
Ctrl+Z                           # Suspend current job
```

### Useful One-Liners
```bash
# Find largest files
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# Monitor log in real-time with colors
tail -f /var/log/syslog | grep --color -E 'error|warning|$'

# Check which process is using most memory
ps aux --sort=-%mem | head -n 10

# Quick backup
tar -czf backup-$(date +%Y%m%d).tar.gz /path/to/dir

# Count total lines of code in project
find . -name "*.py" | xargs wc -l

# Find and replace in multiple files
find . -name "*.conf" -exec sed -i 's/old/new/g' {} \;
```

## Practice Exercises

1. **Basic Navigation**: Navigate to `/var/log`, list files by size, view the last 50 lines of syslog
2. **Log Analysis**: Find all ERROR entries in a log file and count them
3. **Disk Cleanup**: Find all files larger than 100MB modified more than 30 days ago
4. **Process Management**: Find all nginx processes and check their memory usage
5. **Text Processing**: Extract all unique IP addresses from an access log and count occurrences
6. **Automation**: Create an alias that shows disk usage and top 10 largest directories

## Next Steps

- Learn **bash scripting** to automate these commands
- Master **vim** or **nano** for text editing
- Study **Linux file permissions** deeply
- Explore **Docker** and **Kubernetes** commands
- Learn **Git** for version control
- Practice on real servers or use **VirtualBox/Vagrant** for labs

---

**Remember**: The key to mastery is practice. Try to use these commands daily in real scenarios!