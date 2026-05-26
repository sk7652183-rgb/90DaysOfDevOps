Day 11 – File Ownership Challenge (chown & chgrp)

*******Task 1: Understanding Ownership*************

Executed ls -l in the home directory.

<img width="1864" height="141" alt="image" src="https://github.com/user-attachments/assets/fc21d5c7-073f-4619-8b09-a40544899f4c" />

Identified the owner and group columns in Screenshot 3 — the 3rd column represents the user, and the 4th column represents the group.

Verified file ownership

<img width="1920" height="332" alt="image" src="https://github.com/user-attachments/assets/eca91789-9e2f-4c5f-bafe-77f0a07b818f" />

1. Owner (User)

The owner is usually the user who created the file.

Has permissions specifically assigned to them.
Shown in the 3rd column of ls -l.

2. Group

A group is a collection of users.

Multiple users can belong to the same group.
Files can be shared among users through group permissions.
Shown in the 4th column of ls -l.

****Task 2: Basic chown Operations*********************

Changed the file owner to tokyo

<img width="1920" height="635" alt="image" src="https://github.com/user-attachments/assets/b41a8221-ecaf-4cf3-808a-6a2f13867cd4" />

Changed the file owner to berlin

<img width="1920" height="1071" alt="image" src="https://github.com/user-attachments/assets/a32f6dd5-2c3f-4b54-94b4-aed0e379aee1" />

*************Task 3: Basic chgrp Operations*********

Created a file named team-notes.txt

<img width="1920" height="1071" alt="image" src="https://github.com/user-attachments/assets/1355849a-490d-4a3c-90f4-bdf1ba5d6de9" />

Checked the current group of team-notes.txt using ls -l. The current group is ubuntu

<img width="1920" height="157" alt="image" src="https://github.com/user-attachments/assets/55919f0b-5470-4568-b6e9-23b048dc1b4c" />


Created the group heist-team using sudo groupadd heist-team

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/916df4ed-fa4d-40b6-8014-6873a9973e6b" />

Changed the file's group to heist-team and verified the change using ls -l

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5d63d179-b23e-4fd2-a481-213167894812" />

***********Task 4: Combined Owner & Group Change********

Created a file named project-config.yaml

<img width="1920" height="498" alt="image" src="https://github.com/user-attachments/assets/e4c8281c-6248-4868-9801-fc275cb76006" />

Changed the owner to professor and the group to heist-team

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1b740835-7cbe-40a3-8e50-26f445628f0f" />

Created a directory named app-logs/

<img width="1920" height="605" alt="image" src="https://github.com/user-attachments/assets/3eeb6670-42f9-4721-ba98-c5defdbd481a" />

Changed its owner to berlin and group to heist-team using the command sudo chown berlin:heist-team app-logs

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/883f9e05-c440-401e-8350-cfc3962b3eb9" />

***Task 5: Recursive Ownership*****

Created the following directory structure:

mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf

Created the group planners using sudo groupadd planners

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/589aaa54-ae00-48f0-8195-f4cd74779d29" />

Changed the ownership of the entire heist-project/ directory: Owner: professor, Group: planners, using the recursive -R flag

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eb7269a0-2ab4-4c5d-b084-259034a165f8" />

Verified the ownership changes for all files and subdirectories using ls -lR heist-project/

<img width="1861" height="495" alt="image" src="https://github.com/user-attachments/assets/c5c2a50e-62b3-4b52-b822-a30dcca60b3f" />

***************Task 6: Practice Challenge****************

Created the users tokyo, berlin, and nairobi

<img width="1861" height="495" alt="image" src="https://github.com/user-attachments/assets/2e692cba-cd80-49cf-a81b-527f585963da" />

Created the groups vault-team and tech-team

<img width="1861" height="1070" alt="image" src="https://github.com/user-attachments/assets/2f9668f4-4378-48aa-bc4a-c4905dac5230" />

Created directory bank-heist using the mkdir command

Create 3 files inside

touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt



<img width="1859" height="259" alt="image" src="https://github.com/user-attachments/assets/7f9d64dc-0dc4-4a1f-aa83-b725c1798565" />

Set different ownership:

<img width="1859" height="1074" alt="image" src="https://github.com/user-attachments/assets/900ea359-9177-499b-9467-85942e82b154" />




































