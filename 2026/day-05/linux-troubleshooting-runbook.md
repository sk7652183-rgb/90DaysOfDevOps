************Run and record output for at least 8 commands (save snippets in your runbook)********
Environment basics
uname -a displays complete system information, including the Linux kernel version.
<img width="1856" height="210" alt="image" src="https://github.com/user-attachments/assets/a32612f1-fdac-4ed3-a5b6-7d984ed5e71f" />
cat /etc/os-release is used to display Linux distribution and operating system version information.
<img width="1920" height="342" alt="image" src="https://github.com/user-attachments/assets/b9ba9855-39ea-4e91-914a-7b7f79dddff9" />

*******************************************************Filesystem sanity*****************************************************
mkdir /tmp/runbook-demo creates a directory named runbook-demo inside the /tmp folder.
<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/d6303b8c-0571-4775-9f7a-855db18dcf17" />
cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo copies the hosts file to the runbook-demo directory with the name hosts-copy, then lists the contents of that directory.
<img width="1920" height="556" alt="image" src="https://github.com/user-attachments/assets/0b821e44-f6ae-4685-a714-c9d22a23529c" />

*************CPU / Memory******************************************************
top shows real-time running processes and system usage (CPU, memory).
<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/eae9712d-5dab-45cd-858e-2f41b6b4edf1" />
htop is real-time process monitoring command used to view running processes, CPU usage, memory usage, and system performance interactively
<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/b2252f66-2650-4a3f-a849-c0c8333ac3bc" />
ps -o pid is used to display only the Process ID (PID) of running processes.
<img width="1917" height="126" alt="image" src="https://github.com/user-attachments/assets/b711f719-fac3-429d-a7eb-f7229639d67a" />
free -m displays system memory usage information in MB (megabytes).
<img width="1861" height="128" alt="image" src="https://github.com/user-attachments/assets/ccd2c821-a050-43cb-8c70-c4ef5342e706" />
*****************Disk / IO*******************************************************************************************************************
df -h → displays disk space usage of file systems in human-readable format.
du -sh /var/log → shows the total size of the /var/log directory in human-readable format.
<img width="1920" height="385" alt="image" src="https://github.com/user-attachments/assets/8d859231-2b0e-4406-9864-76d5c556f46b" />
iostat is used to monitor CPU usage and disk input/output (I/O) performance statistics in Linux.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e483ff85-f981-45e2-9302-ca86c7c4ce39" />
****************************************Network**********************************************************************************************
netstat -tulpn is used to display all listening network ports and the processes using them.
<img width="1862" height="572" alt="image" src="https://github.com/user-attachments/assets/896aa6c8-ba29-451e-8c32-fed912dec775" />
curl -I google.com is used to fetch only the HTTP response headers from a website (without downloading the page content).
<img width="1867" height="310" alt="image" src="https://github.com/user-attachments/assets/af03486e-9dfa-4415-9bd7-5222de7c9678" />

******************************************************************Logs********************************************************
journalctl -u cron — View logs for a specific systemd service
<img width="1859" height="969" alt="image" src="https://github.com/user-attachments/assets/97f21726-f4e0-4e1f-ba14-c8eff1715c21" />
tail -n 50 /var/log/auth.log is used to display the last 50 lines of the authentication log file.
<img width="1919" height="1080" alt="image" src="https://github.com/user-attachments/assets/55bb23a5-6989-43fa-96e3-e5bee6ef641b" />


*********************one target service/process **********************
systemctl status docker is used to Check if Docker service is running 
docker ps -a List all containers (including stopped)
<img width="1919" height="1080" alt="image" src="https://github.com/user-attachments/assets/f1ccbfd0-ac4a-4599-b7b4-1b23ab75835a" />









