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
![](

