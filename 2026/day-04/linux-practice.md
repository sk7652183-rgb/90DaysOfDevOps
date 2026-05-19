Run and record output for at least 6 commands
***************2 process commands (ps, top, pgrep, etc.)*******************************************
ps --- ps shows a snapshot of currently running processes.
<img width="1862" height="240" alt="image" src="https://github.com/user-attachments/assets/e0589c9a-371f-48cb-a8f3-4756f75949f3" />
top ---- top shows real-time running processes and system usage (CPU, memory).
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4e8fe324-69d6-46b2-9e08-e0a2ff4e58fd" />
pgrep is used to find process IDs (PID) of running programs by their name.
<img width="1860" height="245" alt="image" src="https://github.com/user-attachments/assets/425c41fb-e010-4bd1-9873-c9f8c602c1c0" />
*******************service commands (systemctl status, systemctl list-units, etc.)*****************************************************
systemctl status service_name - Check status of a service
<img width="1914" height="782" alt="image" src="https://github.com/user-attachments/assets/cf786253-6b31-4594-bb07-e4a15af81621" />
systemctl list-units is used to display all active systemd units running on a Linux system
<img width="1913" height="1076" alt="image" src="https://github.com/user-attachments/assets/df0898fb-3947-4a6f-b451-d58543c15afd" />

*******************************log commands (journalctl -u <service>, tail -n 50, etc.)*******************************************************
journalctl -u <service> is used to view logs for a specific systemd service in Linux.
<img width="1913" height="1078" alt="image" src="https://github.com/user-attachments/assets/595c7e83-c9c7-4720-90d4-a200a7d6184c" />
tail -n 5 file_name ---- Displays the last 5 lines of a file.Used to quickly view recent log entries.
<img width="1859" height="204" alt="image" src="https://github.com/user-attachments/assets/8787e43e-df5d-4c99-90b7-b2cb37a32720" />
*******************one service on your system (example: ssh, cron, docker) and inspect it*************************************************************************
systemctl status cron , journalctl -u cron
<img width="1915" height="1076" alt="image" src="https://github.com/user-attachments/assets/357e8edf-f449-4a59-b57f-0f7e1296a696" />
systemctl is-enabled -------Checks whether a service starts automatically at boot.
cron, pgrep cron------------Finds PID of cron service process.
<img width="1855" height="188" alt="image" src="https://github.com/user-attachments/assets/c9c5251a-1983-4925-a977-46a8341b583a" />



