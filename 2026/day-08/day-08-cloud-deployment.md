**********Part 1: Launch Cloud Instance & SSH Access**************

Launched an EC2 instance on AWS. ✅

Command - ssh -i "Tkey.pem" ubuntu@ec2-44-252-65-149.us-west-2.compute.amazonaws.com


<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/8571ee8b-9a40-43e7-99cb-279407fdb83a" />


Successfully connected using SSH.


<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/c0887685-fea9-4431-9c19-57513abd8780" />


**********Part 2: Install Docker & Nginx***************************

Updated the launched instance using the APT package manager.

Command- sudo apt-get update 

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/f8e16f6d-b066-46aa-b3ab-338a4c5f3af6" />

Installed Nginx using the command sudo apt-get install nginx. ✅

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/634b625b-e861-408c-a5f1-ca467441fa85" />

Verified the Nginx service status using systemctl status nginx. ✅

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/ebe70dc8-b31a-4be6-bc45-22bc7f987ba7" />

**************Part 3: Security Group Configuration************

Tested web access by opening a browser and visiting http://44.252.65.149:80. ✅

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/37057de7-fa13-4c78-927a-ef660b243686" />

Downloaded the log file to local machine

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/1d5a2932-6546-49a5-8274-d56c54dd33ef" />








