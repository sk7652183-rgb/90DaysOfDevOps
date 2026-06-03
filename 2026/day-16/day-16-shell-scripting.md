Day 16 – Shell Scripting Basics

***Task_1  First Script*****

Created a file hello.sh, added the shebang line #!/bin/bash at the top, printed Hello, DevOps! using echo, made it executable, and ran it.

<img width="1862" height="702" alt="image" src="https://github.com/user-attachments/assets/b7f6f3de-9582-4bd5-a09f-24032123b6b4" />

What happens if you remove the shebang line?

Without the shebang line, the script can still run if it is executed from a Bash shell, but the operating system won't know which interpreter to use when the script is run directly. The shebang explicitly tells the system to use Bash.


****Task 2: Variables******

Created variables.sh, added the variables Name and Role, and printed: 'Hello, I am <NAME> and I am a <ROLE>'

<img width="1862" height="702" alt="image" src="https://github.com/user-attachments/assets/85910264-2fa5-4a4d-b27a-1994fc6fbfff" />

Try using single quotes vs double quotes — what's the difference?

Double quotes allow variable expansion and command substitution, while single quotes treat everything literally. For example, $Name is replaced with its value inside double quotes, but is printed as $Name inside single quotes

****Task 3: User Input with read****
Created dgreet.sh

Read input from the user using read

<img width="1862" height="1042" alt="image" src="https://github.com/user-attachments/assets/964f8c91-77e9-4a31-9c25-21e430fb0d63" />

****Task 4: If-Else Conditions*****

Created check_number.sh :

Took a number as input using read

Printed whether it is positive, negative, or zero

<img width="1862" height="1042" alt="image" src="https://github.com/user-attachments/assets/033b4739-d7bd-4168-ac0a-2239315f1c77" />

Created file_check.sh that:

Asked for a filename."
Checked if the file exists using -f.
Printed an appropriate message

<img width="1862" height="1042" alt="image" src="https://github.com/user-attachments/assets/790f55e0-6dee-4013-86f5-eec4ae560401" />

***Task 5: Combine It All****

Created server_check.sh that:

Stored the service name nginx in a variable.
Asked the user: "Do you want to check the status? (y/n)"
If the user entered "y", ran systemctl status <service> and displayed the service status.
If the user entered "n", printed "Skipped."

<img width="1862" height="1042" alt="image" src="https://github.com/user-attachments/assets/f1c24270-1ce0-4345-8eef-43e92337c5d6" />



