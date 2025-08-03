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

