**BASIC LINUX COMMANDS**

**FILES & NAVIGATING**

• ls – directory listing (list all files/folders on current dir)

• ls -l – formatted listing

• ls -la – formatted listing including hidden files

• cd dir – change directory to dir (dir will be directory name)

• cd .. – change to parent directory

• cd ./dir – change to dir in parent directory

• cd ~ – change to home directory

• pwd – show current directory

• mkdir dir – create a directory dir

• rm file – delete file

• rm -f dir – force remove file

• rm -rf dir – delete directory dir

• rmdir dir – remove directory dir

• rm -rf / – launch some nuclear bombs targeting your system

• cp file1 file2 – copy file1 to file2

• mv file1 file2 – rename file1 to file2

• mv file dir/file2 – move file to dir as file2

• touch file – create or update file

• cat file – output contents of file

• cat > file – write standard input into file

• cat >> file – append standard input into file

• tail -f file – output contents of file as it grows

**SYSTEM INFO**

• date – show current date/time

• uptime – show uptime

• whoami – who you’re logged in as

• w – display who is online

• cat /proc/cpuinfo – display CPU info

• cat /proc/meminfo – memory info

• free – show memory and swap usage

• du – show directory space usage

• du -sh – displays readable sizes in GB

• df – show disk usage

• uname -a – show kernel config

**NETWORKING**

• ping host – ping host

• whois domain – get whois for domain

• dig domain – get DNS for domain

• dig -x host – reserve lookup host

• wget file – download file

• wget -c file – continue stopped download

• wget -r url – recursively download files from URL

• curl url – outputs the webpage from URL

• curl -o meh.html url – writes the page to meh.html

• ssh user@host – connect to host as user

• ssh -p port user@host – connect using port

• ssh -D user@host – connect & use bind port

**COMPRESSING**

• tar cf file.tar files – tar files into file.tar

• tar xf file.tar – untar into current directory

• tar tf file.tar – show contents of archive

**options**:

• c – create archive

• t – table of contents

• x – extract

• z – use zip/gzip

• f – specify filename

• j – bzip2 compression

• v – verbose

• w – ask for confirmation

• k – do not overwrite

• T – files from file

**PERMISSIONS**

• chmod octal file – change permissions of file

• 4 – read (r)

• 2 – write (w)

• 1 – execute (x)

• **order**: owner/group/world

• chmod 777 – rwx for everyone

• chmod 755 – rw for owner, rx for group world

**PROCESSES**

• ps – display currently active processes

• ps aux – detailed outputs

• kill pid – kill process with process id (pid)

• killall proc – kill all processes named proc

**SOME OTHERS**

• grep pattern file – search in files for pattern

• grep -r pattern dir – search for pattern recursively in dir

• locate file – find all instances of file

• whereis app – show possible locations of app

• man command – show manual page for command

