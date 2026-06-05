Day 17 – Shell Scripting: Loops, Arguments & Error Handling


****Task 1: For Loop******

Create for_loop.sh that: 

Created a loop for a list of 5 fruits and printed each of them.

<img width="1862" height="664" alt="image" src="https://github.com/user-attachments/assets/52a10e12-fbfb-41c0-af11-efd6897dae45" />

Create count.sh Script

Printed numbers 1 to 10 using a for loop

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/15ccd6af-2032-452a-a603-108bfa000c1d" />

*****Task 2: While Loop*************

Created countdown.sh that:"

Took a number from the user.
Counted down to 0 using a while loop.
Printed "Done!" at the end.

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/f308b183-c3f9-4aa2-9c22-8947e2e9ccfa" />


*****Task 3: Command-Line Arguments**************
Created greet.sh that:

Accepted a name as $1.
Printed "Hello, <name>!"
If no argument was passed, printed "Usage: ./greet.sh


<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/3fb8f559-26e5-4862-8c35-22db50883e4a" />

Created args_demo.sh that:

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/62b357d3-be95-4e08-a8cf-d4d0035c3f9d" />

****Task 4: Install Packages via Script****************

Created install_packages.sh that:

Defined a list of packages: nginx, curl, and wget.
Looped through the list.
Checked whether each package was installed using dpkg.
Installed missing packages and skipped those already installed.
Printed the status of each package.

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/a2b45702-3936-44a9-b17d-55ae52b3081b" />


****Task 5: Error Handling**************

Created safe_script.sh that:

Used set -e at the top to exit on errors.
Tried to create the /tmp/devops-test directory.
Tried to navigate into it.
Created a file inside the directory.
Used the || operator to print an error message if any step failed.


Using set -e together with || echo ... means the error is considered "handled" by Bash, so the script may not exit at that line. If the goal is exit  immediately on error, a common pattern is:

mkdir -p /tmp/devops-test || { echo "Failed to create directory"; exit 1; }

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/735fbde8-b73f-4ee0-a935-e148c373635b" />

Modify your install_packages.sh to check if the script is being run as root — exit with a message if not.

<img width="1862" height="1043" alt="image" src="https://github.com/user-attachments/assets/13e7ef0f-6661-442f-9f9e-bb478138a711" />









