# TASK 1
# Linux Essentials & File Permissions Task

## 📌 Task Overview
We will:
1. Create a new user.
2. Build a directory structure.
3. Create a file with specific permissions.
4. Write a backup script with timestamps.

---

## 1️⃣ Create a New User
```bash
sudo adduser studentuser
```
## 2️⃣ Create Directory Structure
```bash
sudo mkdir -p /home/studentuser/projectX/logs
sudo mkdir -p /home/studentuser/projectX/scripts
```
## 3️⃣ Create welcome.txt File
```bash
echo "Welcome to Linux" | sudo tee /home/studentuser/projectX/welcome.txt
```
## 4️⃣ Set File Permissions
```bash
sudo chown studentuser:studentuser /home/studentuser/projectX/welcome.txt
sudo chmod 600 /home/studentuser/projectX/welcome.txt
```
chown → Sets file owner to studentuser.
chmod 600 → Owner: Read/Write, Group: None, Others: None.

## 5️⃣ Create Backup Script
```bash
sudo nano /home/studentuser/projectX/scripts/backup.sh
```
Paste the following into the file:
```bash
#!/bin/bash
cp /home/studentuser/projectX/welcome.txt /home/studentuser/projectX/logs/welcome_$(date +%Y%m%d_%H%M%S).txt
    cp → Copies the file.

    $(date +%Y%m%d_%H%M%S) → Adds timestamp to filename.
```
Save & exit:
Ctrl + O → Save, Enter → Confirm, Ctrl + X → Exit.
## 6️⃣ Make Script Executable
```bash
sudo chmod +x /home/studentuser/projectX/scripts/backup.sh
```
+x → adds execute permission for the owner.
## ✅ Now you can run the backup script:
```bash
/home/studentuser/projectX/scripts/backup.sh
```

![](https://github.com/Mohamedirfan4739/Poc-2/blob/02b9f0b56c90e675283aaf4a985a7eaf62ffe0ec/Screenshot_2025-08-02_19_33_10.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/02b9f0b56c90e675283aaf4a985a7eaf62ffe0ec/Screenshot_2025-08-02_19_33_10.png)

# TASK 2


# 1️⃣ Create script
```bash
nano netinfo.sh
```
# 2️⃣  Bash script (netinfo.sh) that gathers network info, lists open ports, pings Google, and resolves OpenAI’s IP, then writes everything to network_report.txt.
```bash
#!/bin/bash

# File: netinfo.sh

# 1. Display IP address, subnet mask, and default gateway
{
    echo "===== Network Information ====="
    ip addr show | grep -E 'inet ' | awk '{print "IP Address: " $2}'
    echo "Default Gateway:"
    ip route | grep default
    echo

    # 2. List open ports
    echo "===== Open Ports ====="
    if command -v ss &> /dev/null; then
        ss -tuln
    else
        netstat -tuln
    fi
    echo

    # 3. Ping google.com
    echo "===== Ping google.com ====="
    ping -c 4 google.com
    echo

    # 4. DNS Lookup for openai.com
    echo "===== DNS Lookup for openai.com ====="
    nslookup openai.com
} > network_report.txt

echo "Network report saved to network_report.txt"
```
# 3️⃣ Make it executable
```
chmod +x netinfo.sh
```
# 4️⃣ Run it
```
./netinfo.sh
```
4️![](https://github.com/Mohamedirfan4739/Poc-2/blob/29a6591c7c6da14f7f24e1a7f10d482143647479/Screenshot_2025-08-02_20_54_37.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/50dac2e444241353ddc0aefb7b0f12013a5e3785/Screenshot_2025-08-02_20_54_51.png)

# TASK 3
# Mini Server Monitor Script

# 1️⃣ Create script
```
nano monitor.sh
```
# 2️⃣ Bash script for mini server monitor task.
```
#!/bin/bash
# File: monitor.sh
# Description: Checks nginx status, shows system metrics, and logs results.

LOGFILE="/var/log/monitor.log"
TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")

{
    echo "===== $TIMESTAMP ====="

    # 1. Check if nginx is running
    if systemctl is-active --quiet nginx; then
        echo "[INFO] nginx is running."
    else
        echo "[WARN] nginx is NOT running. Attempting to start..."
        systemctl start nginx
        if systemctl is-active --quiet nginx; then
            echo "[SUCCESS] nginx started successfully."
        else
            echo "[ERROR] Failed to start nginx."
        fi
    fi

    # 2. Display memory usage
    echo "Memory Usage:"
    free -h

    # 3. Display CPU load
    echo "CPU Load:"
    uptime

    # 4. Display disk usage
    echo "Disk Usage:"
    df -h

    echo
} >> "$LOGFILE" 2>&1
```
# 3️⃣ Make it executable
```
chmod +x monitor.sh
```
# 4️⃣ Ensure log directory exists
```
sudo mkdir -p /var/log
sudo touch /var/log/monitor.log
sudo chmod 666 /var/log/monitor.log
```
# 5️⃣ Schedule it via cron to run every 5 minutes.
```
crontab -e
```
Add:
```
*/5 * * * * /path/to/monitor.sh
```
![](https://github.com/Mohamedirfan4739/Poc-2/blob/cc2dac41f2881dae47e2c5f6a6edd1ff485e1ecd/Screenshot_2025-08-02_21_29_55.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/96e8f56ae892fa01eba4fb265ebe6e7438796de8/Screenshot_2025-08-02_21_27_57.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/f854dbf805b5f4401cb6fe179b78f41b85750130/Screenshot_2025-08-02_21_30_05.png)

# Task 4: File Watcher Script

# 1️⃣ Install inotify-tools if not installed
```
sudo apt-get install inotify-tools -y
```
# 2️⃣ Create the script
```
nano watch_dir.sh
```
# 3️⃣ Bash script for that will monitor /home/studentuser/projectX/logs for new .txt files and log them with a timestamp.
```
#!/bin/bash
# File: watch_dir.sh
# Description: Watches for new .txt files and logs their names with timestamps.

WATCH_DIR="/home/studentuser/projectX/logs"
LOG_FILE="log_monitor.txt"

# Ensure log file exists
touch "$LOG_FILE"

echo "Watching directory: $WATCH_DIR for new .txt files..."
inotifywait -m -e create --format '%f' "$WATCH_DIR" | while read NEW_FILE
do
    if [[ "$NEW_FILE" == *.txt ]]; then
        TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
        echo "[$TIMESTAMP] New file detected: $NEW_FILE" | tee -a "$LOG_FILE"
    fi
done
```
# 4️⃣ Make it executable
```
chmod +x watch_dir.sh
```
Run the Script:
```
./watch_dir.sh
```
In another terminal window (while the script keeps running), create a .txt file in the watched directory:
```
touch /home/studentuser/projectX/logs/testfile.txt
```
You should see something like this appear in your first terminal:
```
[2025-08-03 09:45:20] New file detected: testfile.txt
```
![](https://github.com/Mohamedirfan4739/Poc-2/blob/0497a158f699c0e304fb259b348325f81b4b2791/Screenshot_2025-08-03_09_25_48.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/3254f1b8e3619bb773ffc8e2a56b21238a60bfea/Screenshot_2025-08-03_09_45_42.png)
![](https://github.com/Mohamedirfan4739/Poc-2/blob/58b8e00b6e6a5393bd6376c2911fc5e79222d4d6/Screenshot_2025-08-03_09_45_52.png)

# Task 5: SSH Login Audit

# 1️⃣ Create script
```
nano ssh_audit.sh
```
# 2️⃣ Bash script that fulfills Task 5.
```
#!/bin/bash
# File: ssh_audit.sh
# Description: Shows last 5 successful and failed SSH logins and saves to ssh_audit.txt.

OUTPUT_FILE="ssh_audit.txt"

{
    echo "===== SSH Login Audit ====="
    echo "Date: $(date)"
    echo

    echo "Last 5 Successful Logins:"
    if [ -f /var/log/auth.log ]; then
        grep "Accepted" /var/log/auth.log | tail -n 5
    else
        journalctl _COMM=sshd | grep "Accepted" | tail -n 5
    fi
    echo

    echo "Last 5 Failed Login Attempts:"
    if [ -f /var/log/auth.log ]; then
        grep "Failed" /var/log/auth.log | tail -n 5
    else
        journalctl _COMM=sshd | grep "Failed" | tail -n 5
    fi
} > "$OUTPUT_FILE"

echo "SSH audit results saved to $OUTPUT_FILE"
```
# 3️⃣ Make it executable
```
chmod +x ssh_audit.sh
```
# 4️⃣ Run it
```
./ssh_audit.sh
```
![](https://github.com/Mohamedirfan4739/Poc-2/blob/0b620f02c02066fe61abbc9b242107cc0a79ba7b/Screenshot_2025-08-03_10_00_35.png)
![](
