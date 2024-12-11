Bash Scripts

1. Update and Upgrade System Packages
Purpose: Ensure the system is up-to-date with the latest security patches and software updates.

Script:

#!/bin/bash

echo "Updating system packages..."
sudo apt update && sudo apt upgrade -y

echo "System updated successfully!"
Explanation:

apt update refreshes the list of available packages.
apt upgrade -y installs the updated versions of the packages automatically.
sudo is used to run commands with administrative privileges.
2. Monitor Disk Space
Purpose: Check disk usage and alert if usage exceeds a specified threshold.

Script:

#!/bin/bash
THRESHOLD=80
echo "Checking disk usage..."
USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ "$USAGE" -gt "$THRESHOLD" ]; then
  echo "Disk usage is above $THRESHOLD%! Current usage: $USAGE%"
else
  echo "Disk usage is below $THRESHOLD%. Current usage: $USAGE%"
fi
Explanation:

df / checks disk space for the root partition.
awk and sed extract and clean the usage percentage.
The if statement compares the usage to the threshold and prints a warning if exceeded.
3. Automate File Backups
Purpose: Back up a directory to a specific location.

Script:

#!/bin/bash
SOURCE="/path/to/source"
DEST="/path/to/backup"
DATE=$(date +%Y-%m-%d)
echo "Backing up files from $SOURCE to $DEST/$DATE..."
mkdir -p "$DEST/$DATE"
cp -r "$SOURCE"/* "$DEST/$DATE"
echo "Backup completed!"
Explanation:

Replace /path/to/source and /path/to/backup with the actual paths.
date +%Y-%m-%d generates a timestamp for the backup folder.
mkdir -p creates the backup directory if it doesn’t exist.
cp -r recursively copies all files.
4. Deploy a Simple Web Server
Purpose: Quickly set up a web server with Nginx.

Script:

#!/bin/bash
echo "Installing Nginx..."
sudo apt update
sudo apt install -y nginx
echo "Starting Nginx service..."
sudo systemctl start nginx
sudo systemctl enable nginx
echo "Nginx web server is up and running!"
Explanation:

Installs Nginx, a popular web server.
systemctl starts and enables Nginx to run on boot.
5. Add and Manage Users
Purpose: Automate user creation with pre-defined privileges.

Script:

#!/bin/bash
USERNAME="newuser"
PASSWORD="password123"
GROUP="sudo"
echo "Creating user $USERNAME..."
sudo useradd -m -s /bin/bash "$USERNAME"
echo "$USERNAME:$PASSWORD" | sudo chpasswd
echo "Adding $USERNAME to $GROUP group..."
sudo usermod -aG "$GROUP" "$USERNAME"
echo "User $USERNAME created and added to $GROUP!"
Explanation:

useradd creates a new user.
chpasswd sets the user password.
usermod -aG adds the user to a specified group (e.g., sudo).
6. Check Server Uptime
Purpose: Display how long the server has been running.

Script:

#!/bin/bash
echo "Server uptime:"
uptime
Explanation:

uptime shows the server's uptime, load averages, and users logged in.
7. Check Active Processes
Purpose: Identify resource-intensive processes.

Script:

#!/bin/bash
echo "Top 5 processes by CPU usage:"
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -6
Explanation:

ps lists processes with details like PID, CPU, and memory usage.
--sort=-%cpu sorts processes by CPU usage in descending order.
head -6 limits the output to the top 5 processes (plus the header).
8. Automate SSH Key Generation
Purpose: Generate SSH keys for secure remote access.

Script:

#!/bin/bash
KEY_NAME="id_rsa"
echo "Generating SSH key..."
ssh-keygen -t rsa -b 4096 -f "$HOME/.ssh/$KEY_NAME" -N ""
echo "SSH key generated at $HOME/.ssh/$KEY_NAME"
Explanation:

ssh-keygen creates an RSA key pair with 4096 bits.
-f specifies the file name and location.
-N "" sets an empty passphrase.
9. Restart a Service
Purpose: Restart a service if it’s not running.

Script:

#!/bin/bash
SERVICE="nginx"
if ! systemctl is-active --quiet "$SERVICE"; then
  echo "$SERVICE is not running. Restarting..."
  sudo systemctl restart "$SERVICE"
else
  echo "$SERVICE is running."
fi
Explanation:

systemctl is-active checks if the service is running.
If not, the script restarts the service.
10. Schedule a Cron Job

Purpose: Automate task scheduling by adding a cron job.

Script:

#!/bin/bash
JOB="0 2 * * * /path/to/script.sh"
CRON_FILE="/tmp/mycron"
echo "Adding cron job..."
(crontab -l 2>/dev/null; echo "$JOB") | crontab -
echo "Cron job added successfully!"
Explanation:

Replace /path/to/script.sh with the script you want to schedule.
(crontab -l; echo "$JOB") appends the new job to existing ones.
crontab - loads the updated list of cron jobs.
Tips for Running the Scripts:
Save the Script: Save the script as a .sh file (e.g., script.sh).
Make it Executable: Run chmod +x script.sh to make it executable.
Run the Script: Execute it using ./script.sh.
Debugging: Use bash -x script.sh to debug if errors occur.
These scripts provide a solid foundation for automating repetitive tasks in DevOps workflows.