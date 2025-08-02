# Basic Linux Commands

## Files & Navigating

- `ls`  
  List files and folders in the current directory

- `ls -l`  
  Long format listing with permissions, owner, size, and timestamp

- `ls -la`  
  Long format listing including hidden (`.`) files

- `cd <dir>`  
  Change directory to `<dir>`

- `cd ..`  
  Move to parent directory

- `cd ./<dir>`  
  Change to `<dir>` in the current directory

- `cd ~`  
  Change to your home directory

- `pwd`  
  Print the current working directory

- `mkdir <dir>`  
  Create a new directory named `<dir>`

- `rm <file>`  
  Remove the file `<file>`

- `rm -f <file>`  
  Force remove `<file>` without prompt

- `rm -rf <dir>`  
  Recursively remove `<dir>` and its contents

- `rmdir <dir>`  
  Remove empty directory `<dir>`

- `rm -rf /`  
  WARNING: Deletes everything on the system root (never run this)

- `cp <source> <destination>`  
  Copy `<source>` file to `<destination>`

- `mv <source> <destination>`  
  Move or rename `<source>` to `<destination>`

- `mv <file> <dir>/<new-name>`  
  Move `<file>` into `<dir>` with new name `<new-name>`

- `touch <file>`  
  Create an empty file or update its timestamp

- `cat <file>`  
  Display contents of `<file>`

- `cat > <file>`  
  Redirect input into `<file>`, overwriting it

- `cat >> <file>`  
  Redirect input into `<file>`, appending to it

- `tail -f <file>`  
  Follow the end of `<file>` as it is updated

## System Information

- `date`  
  Display the current date and time

- `uptime`  
  Show how long the system has been running

- `whoami`  
  Print the effective username of the current user

- `w`  
  Show who is logged in and their processes

- `cat /proc/cpuinfo`  
  Display detailed CPU information

- `cat /proc/meminfo`  
  Display memory usage information

- `free -h`  
  Display memory and swap usage in human-readable format

- `du`  
  Estimate file and directory space usage

- `du -sh`  
  Summarize disk usage in human-readable format

• df – show disk usage

• uname -a – show kernel config

## Networking

- `ping <host>`  
  Send ICMP ECHO_REQUEST to network hosts

- `whois <domain>`  
  Query the WHOIS database for `<domain>`

- `dig <domain>`  
  DNS lookup for `<domain>`

- `dig -x <ip>`  
  Reverse DNS lookup for `<ip>`

- `wget <url>`  
  Download files from the web

- `wget -c <url>`  
  Continue a partially downloaded file

- `wget -r <url>`  
  Recursively download content from `<url>`

- `curl <url>`  
  Transfer data from `<url>` and display it

- `curl -o <file> <url>`  
  Save output from `<url>` to `<file>`

- `ssh <user>@<host>`  
  Open SSH connection to `<host>` as `<user>`

- `ssh -p <port> <user>@<host>`  
  Specify `<port>` for SSH connection

- `ssh -D <port> <user>@<host>`  
  Create a SOCKS proxy at local `<port>`

## Compressing Archives

- `tar cf <archive>.tar <files>`  
  Create `<archive>.tar` from `<files>`

- `tar xf <archive>.tar`  
  Extract `<archive>.tar` in current directory

- `tar tf <archive>.tar`  
  List contents of `<archive>.tar`

### Common `tar` Options

- `c`  
  Create a new archive

- `t`  
  List the archive contents

- `x`  
  Extract files from archive

- `z`  
  Filter archive through gzip

- `f`  
  Use archive file

- `j`  
  Filter archive through bzip2

- `v`  
  Verbosely list files processed

- `w`  
  Prompt before each file

- `k`  
  Keep existing files on extract

- `T`  
  Get list of files from `<file>`

## File Permissions

- `chmod <mode> <file>`  
  Change file permissions to `<mode>`

- `4`  
  Read permission

- `2`  
  Write permission

- `1`  
  Execute permission

- Permissions order: owner/group/others

- `chmod 777 <file>`  
  Full permissions for all users

- `chmod 755 <file>`  
  Owner can rwx, group/others can rx

## Process Management

- `ps`  
  List current shell processes

- `ps aux`  
  Show all processes in detailed format

- `kill <pid>`  
  Send TERM signal to `<pid>`

- `killall <process>`  
  Kill all processes named `<process>`

## Other Useful Commands

- `grep <pattern> <file>`  
  Search for `<pattern>` in `<file>`

- `grep -r <pattern> <dir>`  
  Recursively search `<pattern>` in `<dir>`

- `locate <file>`  
  Find all occurrences of `<file>` in database

- `whereis <app>`  
  Locate binary, source, and manual page for `<app>`

- `man <command>`  
  Display the manual page for `<command>`

