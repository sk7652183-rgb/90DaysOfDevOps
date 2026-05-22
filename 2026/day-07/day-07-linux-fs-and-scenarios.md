****************Linux File System Hierarchy**************

/ ------------Starting point of the system — everything starts here

/home -----------Stores home directories for regular users

/root ----------- Stores files and configuration for the root user

/etc ----------- Stores configuration files for the system and applications

/var/log ----------- Stores system and application logs, important for DevOps engineers during monitoring and issue investigation

/tmp ----------- Stores temporary files created by the system and applications

*************Additional Directories*****************************

/bin ----------- Stores essential executable commands required for system operation,Essential commands needed for boot/basic operation

/usr/bin ----------- Contains user executable commands and applications

/opt ----------- Used for installing add-on or third-party applications in separate directories

ls -l   # Lists files in long format with permissions, ownership, size, and modification time

<img width="1920" height="1051" alt="image" src="https://github.com/user-attachments/assets/f2f056ec-1c4f-440a-aa0f-77d606cb6d64" /> 

# Find the largest log file in /var/log

du -sh /var/log/*
→ Shows disk usage of each file/folder inside /var/log in human-readable format

2>/dev/null
→ Suppresses error messages (like permission denied)

sort -h
→ Sorts results by size (human-readable sorting: K, M, G)

tail -5
→ Shows the 5 largest items


<img width="1920" height="844" alt="image" src="https://github.com/user-attachments/assets/9282c2c3-b3bf-475c-98b1-700030918a13" />

# Look at a config file in /etc

cat /etc/hostname---
This command displays the hostname of the system by reading the /etc/hostname file.


<img width="1919" height="886" alt="image" src="https://github.com/user-attachments/assets/f2e41795-b5b0-48af-b851-74cd108b027f" />

ls -la   # Lists all files and directories, including hidden files, in long format

<img width="1920" height="1046" alt="image" src="https://github.com/user-attachments/assets/a5906f12-b72b-4c1e-829f-2805fc38f8fe" />

Example Scenario: Check if a service is running

Question: How do you check if the 'nginx' service is running?


systemctl status nginx

The command `systemctl status nginx` is used to check the current status of the Nginx service.

While running this command, I encountered an issue:  
`ConditionFileIsExecutable=/usr/sbin/nginx was not met`.

This indicated that the Nginx executable was missing or not available at the expected path, so the service could not start.

I investigated the issue and found that the binary was not correctly present. I tried enabling and starting the service, but it was unsuccessful. Finally, I resolved the issue by reinstalling the Nginx package.

<img width="1920" height="1046" alt="image" src="https://github.com/user-attachments/assets/4a1ee13c-c0ab-4619-9e4e-2787dd9e0d81" />

<img width="1920" height="914" alt="image" src="https://github.com/user-attachments/assets/07e4f9aa-0622-4239-b33e-4fbdb4a12935" />

systemctl list-units --type=service

This command lists all systemd service units that are currently loaded in the system, including their active/inactive state.

<img width="1919" height="1075" alt="image" src="https://github.com/user-attachments/assets/1408767a-c5fe-4cb2-82f2-3cd6721a38c3" />


systemctl is-enabled nginx

This command checks if the Nginx service is enabled to start automatically at system boot.

Scenario 1: Service Not Starting

A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.

I checked the service status using the following commands:

- sudo systemctl status nginx → to view the current status of the service  
- systemctl is-enabled nginx → to check whether the service is enabled at boot  
- journalctl -u nginx -n 50 → to view the last 50 log entries for the service

<img width="1919" height="1075" alt="image" src="https://github.com/user-attachments/assets/df89c88b-29f5-4c39-8285-65e7c0b08c71" />

Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU?

The command `ps aux --sort=-%cpu | head -10` is used to display the top 10 processes consuming the highest CPU resources.

<img width="1860" height="936" alt="image" src="https://github.com/user-attachments/assets/5b38e2d6-6914-4f49-8d0a-a82b73211151" />

A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?

journalctl -u docker -n 50

<img width="1916" height="1074" alt="image" src="https://github.com/user-attachments/assets/dbb641a2-933a-435a-bafb-92cdaca8b725" />


systemctl status ssh
Shows current state of the SSH service (running, inactive, failed), along with recent logs.

journalctl -u ssh -n 50
Displays the last 50 logs related to the SSH service for troubleshooting.

journalctl -u ssh -f
Continuously streams live logs of the SSH service (useful for debugging live issues).

<img width="1916" height="1074" alt="image" src="https://github.com/user-attachments/assets/a28ef576-b822-44e7-8c22-f213fcb965e1" />


<img width="1916" height="1074" alt="image" src="https://github.com/user-attachments/assets/44a23e1a-7dc0-4340-b87d-3ffb33a11729" />


A script at /home/user/backup.sh is not executing.
When you run it: ./backup.sh
You get: "Permission denied"

What commands would you use to fix this?

vim backup.sh Created a script file for testing purposes.

ls -l to Lists files in long format, including permissions, owner, group, and size.

chmod +x backup.sh Gives execute permission to the script.

./backup.sh Runs the script from the current directory.


<img width="1916" height="1074" alt="image" src="https://github.com/user-attachments/assets/2c28ce26-aed0-4d5f-849c-3f059c38395c" />









