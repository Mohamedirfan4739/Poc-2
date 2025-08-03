# Task 6: Crontab Practice

# 1.You can edit the crontab with:
```
crontab -e
```
# 2.Then add these lines:
Print "Good morning!" every day at 8 AM
```
0 8 * * * echo "Good morning!"
```
Backup /home/studentuser/projectX/ every Sunday
Here we’ll make a tar archive in /home/studentuser/backups/ with a timestamp:
```
0 0 * * 0 tar -czf /home/studentuser/backups/projectX_$(date +\%F).tar.gz /home/studentuser/projectX/
```
Delete .log files older than 7 days every Friday at midnight
````
0 0 * * 5 find /path/to/logs -name "*.log" -type f -mtime +7 -exec rm -f {} \;
````
![](https://github.com/Mohamedirfan4739/Poc-2/blob/2413def1c96d169a186c6dec9bfc5cb5ab299406/Screenshot_2025-08-03_17_10_09.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/0daa07785b20fc8d98218559f1f9d0df7fb18cda/Screenshot_2025-08-03_17_10_01.png)

# Task 7: Port Scanner Script

# 1.Create the script file
```
nano portscan.sh
```
# 2.Paste the bash script:
```
#!/bin/bash

# Check if IP address is supplied
if [ -z "$1" ]; then
    echo "Usage: $0 <IP_ADDRESS>"
    exit 1
fi

IP=$1

echo "Scanning ports 20-25 on $IP..."
for PORT in {20..25}; do
    timeout 1 bash -c "echo > /dev/tcp/$IP/$PORT" 2>/dev/null &&
        echo "Port $PORT is OPEN" ||
        echo "Port $PORT is CLOSED"
done
```
# 3.Make it executable:
```
chmod +x portscan.sh
```
# 4.Run it with:
```
./portscan.sh 192.168.1.10
```
![](https://github.com/Mohamedirfan4739/Poc-2/blob/47bbe5f8bf9baca7c60221bc9880ecf19214d997/Screenshot_2025-08-03_17_23_22.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/956a276b58240e547663c30771fe1a94e5110a55/Screenshot_2025-08-03_17_22_46.png)

# Task 8: Website Availability Checker

# Step 1 – Create the sites.txt file

This file will contain the websites you want to check.
```
nano sites.txt
```
Inside, add one website per line:Step 1 – Create the sites.txt file

This file will contain the websites you want to check.

nano sites.txt

Inside, add one website per line:

https://www.google.com
https://www.github.com
https://example.com

Save and exit:

    Press CTRL+O → Enter → CTRL+X.

Step 2 – Create the script

We’ll make a script file called site_checker.sh.

nano site_checker.sh

Paste this script:

#!/bin/bash

INPUT_FILE="sites.txt"
LOG_FILE="site_status.log"

# Check if sites.txt exists
if [ ! -f "$INPUT_FILE" ]; then
    echo "Error: $INPUT_FILE not found!"
    exit 1
fi

# Clear old log file
> "$LOG_FILE"

# Loop through each site in sites.txt
while read -r SITE; do
    if [ -n "$SITE" ]; then
        STATUS_CODE=$(curl -s -o /dev/null -w "%{http_code}" "$SITE")
        if [ "$STATUS_CODE" -eq 200 ]; then
            echo "$(date '+%Y-%m-%d %H:%M:%S') - $SITE is UP (Status: $STATUS_CODE)" | tee -a "$LOG_FILE"
        else
            echo "$(date '+%Y-%m-%d %H:%M:%S') - $SITE is DOWN (Status: $STATUS_CODE)" | tee -a "$LOG_FILE"
        fi
    fi
done < "$INPUT_FILE"

Save and exit.
Step 3 – Make the script executable

Run this command so you can execute it:

chmod +x site_checker.sh

Step 4 – Run the script

./site_checker.sh

Step 5 – Check the results

    The results will also appear on the screen.

    They will be saved in site_status.log:

cat site_status.log

```
https://www.google.com
https://www.github.com
https://example.com
```
Save and exit:

Press CTRL+O → Enter → CTRL+X.

# Step 2 – Create the script

We’ll make a script file called site_checker.sh.
```
nano site_checker.sh
```
Paste this script:
```
#!/bin/bash

INPUT_FILE="sites.txt"
LOG_FILE="site_status.log"

# Check if sites.txt exists
if [ ! -f "$INPUT_FILE" ]; then
    echo "Error: $INPUT_FILE not found!"
    exit 1
fi

# Clear old log file
> "$LOG_FILE"

# Loop through each site in sites.txt
while read -r SITE; do
    if [ -n "$SITE" ]; then
        STATUS_CODE=$(curl -s -o /dev/null -w "%{http_code}" "$SITE")
        if [ "$STATUS_CODE" -eq 200 ]; then
            echo "$(date '+%Y-%m-%d %H:%M:%S') - $SITE is UP (Status: $STATUS_CODE)" | tee -a "$LOG_FILE"
        else
            echo "$(date '+%Y-%m-%d %H:%M:%S') - $SITE is DOWN (Status: $STATUS_CODE)" | tee -a "$LOG_FILE"
        fi
    fi
done < "$INPUT_FILE"

Save and exit.
Step 3 – Make the script executable
```
Run this command so you can execute it:
```
chmod +x site_checker.sh
```
# Step 4 – Run the script
```
./site_checker.sh
```
# Step 5 – Check the results

The results will also appear on the screen.

They will be saved in site_status.log:
```
cat site_status.log
```
![](https://github.com/Mohamedirfan4739/Poc-2/blob/757d369de20f19ffc561d29cecfbcf1456b675a5/Screenshot_2025-08-03_17_36_39.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/b317964abefd93c191d4cad07480285e985f93f8/Screenshot_2025-08-03_17_40_25.png)

# Task 9: Environment and Disk Report

# Step 1 – Create the script file

We’ll call it env_disk_report.sh:
```
nano env_disk_report.sh
```
# Step 2 – Add the script content

Paste this inside:
```
#!/bin/bash

# Report file name with timestamp
REPORT="env_disk_report_$(date +%F_%H-%M-%S).txt"

# 1. Current user, hostname, and uptime
echo "===== USER, HOSTNAME & UPTIME =====" >> "$REPORT"
echo "User: $(whoami)" >> "$REPORT"
echo "Hostname: $(hostname)" >> "$REPORT"
echo "Uptime: $(uptime -p)" >> "$REPORT"
echo "" >> "$REPORT"

# 2. Mounted filesystems and usage
echo "===== MOUNTED FILESYSTEMS & USAGE =====" >> "$REPORT"
df -h >> "$REPORT"
echo "" >> "$REPORT"

# 3. PATH and shell-related environment variables
echo "===== ENVIRONMENT VARIABLES =====" >> "$REPORT"
echo "PATH: $PATH" >> "$REPORT"
echo "Shell: $SHELL" >> "$REPORT"
echo "Home: $HOME" >> "$REPORT"
echo "" >> "$REPORT"

# Done message
echo "Report generated: $REPORT"
```
# Step 3 – Save and exit

In nano:

Press CTRL+O → Enter → CTRL+X.

# Step 4 – Make it executable
```
chmod +x env_disk_report.sh
```
# Step 5 – Run the script
```
./env_disk_report.sh
```
# Step 6 – View the report

It will create a file like:

env_disk_report_2025-08-03_14-30-00.txt

Check its contents:
```
cat env_disk_report_2025-08-03_14-30-00.txt
```
Example output:

===== USER, HOSTNAME & UPTIME =====
User: studentuser
Hostname: my-computer
Uptime: up 2 hours, 14 minutes

===== MOUNTED FILESYSTEMS & USAGE =====
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /

===== ENVIRONMENT VARIABLES =====
PATH: /usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin
Shell: /bin/bash
Home: /home/studentuser

