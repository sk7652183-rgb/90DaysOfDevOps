Day 12 – Breather & Revision (Days 01–11)

Mindset & plan

I'm still aligned with the goals I set on Day 01. However, I need more hands-on practice to build stronger confidence in both my fundamentals and advanced skills

*****Processes & services****

rerun 2 commands from Day 04/05

"ps command is used to view running processes on the system."
"systemctl status is used to check the status of a service."
"journalctl -u <service> is used to view detailed logs of a service."

<img width="1859" height="1074" alt="image" src="https://github.com/user-attachments/assets/0d0cbae9-2c67-4561-9b61-3f3867afe686" />

File skills: practice 3 quick ops from Days 06–11

echo  " >> filename is used to append content to a file. If the file does not exist, it creates the file and then appends the content."

ls -l | grep -i test123 is used to list files in long format and filter the output using grep to search for the pattern test123 in a case-insensitive manner

chmod is used to change file permissions. The default permission of the file was 664 when it was created. I changed the file permission to 700

<img width="1918" height="247" alt="image" src="https://github.com/user-attachments/assets/9436fe30-1272-489f-bef9-cbb3d0c2a01d" />

chown is used to change the owner and/or group ownership of a file or directory

<img width="1918" height="1070" alt="image" src="https://github.com/user-attachments/assets/2319c5c5-71dc-4746-9779-b17b503a7a9c" />

Recreated a small scenario from Day 09/Day 11 by creating a user and changing file ownership. Verified the changes using ls -l

<img width="1918" height="1070" alt="image" src="https://github.com/user-attachments/assets/68c723d9-06f1-4d65-9ac1-b18f9e560da8" />

*****Mini Self-Check (write short answers in day-12-revision.md)***

1.Which 3 commands save you the most time right now, and why?

ls -la is the command I use it to view file details in long format and display hidden files

touch command use it to create new files, especially during automation tasks and scripting.

free -h use it to check memory usage in human-readable format.

2.How do you check if a service is healthy? List the exact 2–3 commands you’d run first.

ps aux | grep <service> (or htop) Used to verify whether the service process is running on the system.

systemctl status <service> is used to check the status of a service and see whether it is running, active, inactive, or failed

journalctl -u <service> is used to view detailed logs of a service.

3.How do you safely change ownership and permissions without breaking access? Give one example command

To safely change ownership and permissions without breaking access, first verify the current owner, group, and permissions using ls -l, then apply only the required changes.

4.What will you focus on improving in the next 3 days?

In the next 3 days, I will mainly focus on automation and scripting while becoming more confident in Linux basics

