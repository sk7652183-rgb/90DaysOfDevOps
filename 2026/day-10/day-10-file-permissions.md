************Day 10 – File Permissions & File Operations Challenge***************

Task 1: Creates Files

Created an empty file named devops.txt using the touch command

Created notes.txt with some content using echo

Created script.sh using vim and added the content: echo "Hello DevOps"

Verified file permissions using the ls -l command

<img width="1919" height="1080" alt="image" src="https://github.com/user-attachments/assets/2b27a651-74a9-4cf2-aeee-8783ba64ed57" />

************Task 2: Read Files***********

Read notes.txt using the cat command

<img width="1919" height="225" alt="image" src="https://github.com/user-attachments/assets/64952127-5233-4a62-a67e-2cd2151d9a51" />

Viewed script.sh in vim read-only mode.

<img width="1920" height="1077" alt="image" src="https://github.com/user-attachments/assets/cae5469a-2dbe-42b3-a92c-232f933b6fee" />

Displayed the first 5 lines of /etc/passwd using head

Displayed the last 5 lines of /etc/passwd using tail

<img width="1920" height="391" alt="image" src="https://github.com/user-attachments/assets/3b4009d7-7643-4b44-8cae-f3673669d9a9" />

*****Task 3: Understand Permissions*****

Checked the permissions of devops.txt, notes.txt, and script.sh using ls -l

devops.txt has permissions 775, notes.txt has permissions 664, and script.sh has permissions 775

<img width="1920" height="542" alt="image" src="https://github.com/user-attachments/assets/da792bfb-5057-4f12-82b3-d6a3396ba50d" />

*****Modify Permissions****************

Changed devops.txt to read-only by removing write permission for all users using chmod command 

Changed the permissions of notes.txt to 640

Created a directory named project/ with 755 permissions.

Verified the changes using ls -l after each modification

<img width="1920" height="1072" alt="image" src="https://github.com/user-attachments/assets/73e9fa3e-85d1-4f08-b8d3-8de7f83f96b0" />


*******Task 5: Test Permissions*********************

Attempted to write to a read-only file

Result: It displayed the warning: W10: Warning: Changing a readonly file.

Tried executing a file without execute permission

Result : -bash: ./script.sh: Permission denied





