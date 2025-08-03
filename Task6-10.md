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
