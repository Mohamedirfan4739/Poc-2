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
```
    cp → Copies the file.

    $(date +%Y%m%d_%H%M%S) → Adds timestamp to filename.

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




