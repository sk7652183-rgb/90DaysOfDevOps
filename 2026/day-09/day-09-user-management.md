# Day 09 Challenge

********************Task 1: Create Users*********************************************

Created three users along with their home directories and passwords.

Command - sudo useradd -m berlin && echo "berlin:test123" | sudo chpasswd

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/af67d8ba-9d55-47d3-9eca-96bd3075fc29" />

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/26d8c539-bfaf-4978-94e0-70dda0a9eee1" />


*******************Task 2: Create Groups*********************************************************

Created two groups using the sudo groupadd command. ✔️

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/f87cc7a3-fa19-4ead-ade8-a2a9932d72eb" />

********************************Task 3: Assign to Groups*************************

Assigned the user using this command: sudo usermod -aG group username

Assigned the user to multiple groups using this command: sudo usermod -aG group1,group2 username

<img width="1920" height="1079" alt="image" src="https://github.com/user-attachments/assets/34f1f1be-9247-439e-be92-d149a40f8040" />

*******************Task 4: Shared Directory********************************

Created the directory using mkdir -p /opt/dev-project.

<img width="1920" height="253" alt="image" src="https://github.com/user-attachments/assets/c5b6503f-089d-4492-82a2-3c2844411185" />

Changed the group owner to developers using the command sudo chgrp developers /opt/dev-project

<img width="1920" height="495" alt="image" src="https://github.com/user-attachments/assets/ea90f024-5be7-461c-88ed-21aab3466d6a" />

Set the permissions to 775 (rwxrwxr-x) using chmod

<img width="1920" height="265" alt="image" src="https://github.com/user-attachments/assets/431fd163-87bd-4fbc-ba41-3a5c9548fe57" />

Tested by creating files as tokyo and berlin

<img width="1920" height="303" alt="image" src="https://github.com/user-attachments/assets/5b20e652-348b-4680-b470-56985cd96431" />


<img width="1920" height="322" alt="image" src="https://github.com/user-attachments/assets/7791e2a6-b2a2-47aa-b5f2-b477c98e8dd7" />


**************************Task 5: Team Workspace*******************************

✅ Created user nairobi with a home directory.

✅ Created group project-team.

✅ Added nairobi and tokyo to project-team.

✅ Created the /opt/team-workspace directory.

✅ Set the group to project-team and permissions to 775.

✅ Tested by creating a file as nairobi.


<img width="1920" height="1010" alt="image" src="https://github.com/user-attachments/assets/2adc9bf5-2b02-4c9e-8ac1-b6de4879fb66" />

<img width="1920" height="296" alt="image" src="https://github.com/user-attachments/assets/45326850-4686-4f4f-a589-7241b6f9735b" />






