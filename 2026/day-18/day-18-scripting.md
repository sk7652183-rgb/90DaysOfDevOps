Day 18 – Shell Scripting: Functions & intermediate Concepts

***Task 1: Basic Functions************

## Function Example

```bash
#!/bin/bash

# Created a function greet that takes a name as an argument
# and prints "Hello, <name>!"
function greet() {
    echo "Hello, my name is $1"
}

# Created a function add that takes two numbers
# and prints their sum
function add() {
    echo "The addition of two numbers is: $(($1 + $2))"
}

# Called both functions from the script
greet "Abusufiyan"
add 4 5
```

### Output

```text
Hello, my name is Abusufiyan
The addition of two numbers is: 9
```
<img width="1858" height="1052" alt="image" src="https://github.com/user-attachments/assets/d4380703-a344-494d-bfb8-8fe49c9b0b20" />

***************Task 2: Functions with Return Values**************************

## System Health Check Script

```bash
#!/bin/bash

# A function check_disk that checks disk usage of / using df -h
function check_disk() {
    echo "The available disk space on / is:"
    df -h | awk 'NR==3 {print $4}'
}

# A function check_memory that checks free memory using free -h
function check_memory() {
    echo "The available memory is:"
    free -h | awk 'NR==2 {print $7}'
}

# Main section that calls both functions and prints the results
check_disk
check_memory
```

### Output

```text
The available disk space on / is:
55G

The available memory is:
5.8Gi
```


<img width="1858" height="1052" alt="image" src="https://github.com/user-attachments/assets/00db86e2-4f00-4767-bc79-13f90a68af94" />



***************Task 3: Strict Mode — set -euo pipefail****************

Created strict_demo.sh with set -euo pipefail at the top

- `set -u` treats any unset variable as an error.
- In the script, the variable `Username` is referenced before being assigned a value.
- Bash stops execution and displays:

<img width="1858" height="502" alt="image" src="https://github.com/user-attachments/assets/7912fb30-9df8-4e5b-8349-280afa3711a2" />

### Summary

| Option | Purpose |
|----------|---------|
| `set -u` | Exit when an unset variable is used |
| `${VAR:-default}` | Use a default value if variable is unset |
| `${VAR:?message}` | Show custom error if variable is unset |

Try a command that fails — what happens with set -e

The `set -e` option tells Bash to **exit immediately if any command returns a non-zero (failure) status**.

<img width="1857" height="611" alt="image" src="https://github.com/user-attachments/assets/52ce9799-51a4-4db0-806d-ca9e183dba37" />

Try a piped command where one part fails — what happens with set -o pipefail?

set -o pipefail changes how Bash handles pipelines (cmd1 | cmd2 | cmd3).

Normally, Bash returns the exit status of the last command in the pipeline. With pipefail, Bash returns a failure if any command in the pipeline fails.

<img width="1858" height="904" alt="image" src="https://github.com/user-attachments/assets/527dd3a6-ae39-4a23-8844-2d8ca0b699d4" />


Document: What does each flag do?


| Option | Purpose |
|---------|---------|
| `set -e` | Exit immediately if a command fails |
| `set -u` | Exit if an unset variable is used |
| `set -x` | Print commands before executing them |
| `set -o pipefail` | Fail if any command in a pipeline fails |


****Task 4: Local Variables*****

Variables declared with local exist only inside the function.
Regular variables created inside a function are global by default in Bash and remain available after the function finishes.
Using local helps avoid accidental overwriting of variables elsewhere in your script.

=== Using local variables ===
Inside local_function:
Name: Alice
Age: 25

Outside local_function:
Name: Not Defined
Age: Not Defined

=== Using regular variables ===
Inside global_function:
City: New York
Country: USA

Outside global_function:
City: New York
Country: USA

<img width="1858" height="439" alt="image" src="https://github.com/user-attachments/assets/afa1fe6c-d98b-48bc-a85d-d7b16d2aa96d" />

<img width="1858" height="994" alt="image" src="https://github.com/user-attachments/assets/2d52a02e-584e-4f69-85a3-8a9b8882d89b" />

****Task 5: Build a Script — System Info Reporter*****

# System Information Script

This Bash script collects and displays useful system information, including:

* Hostname and OS information
* System uptime
* Top 5 largest files/directories in the current directory
* Memory usage
* Top 5 CPU-consuming processes

## Script

```bash
#!/bin/bash

set -euo pipefail

# Print hostname and OS information
print_system_info() {
    echo "Hostname: $(hostname)"
    echo "OS Information:"
    cat /etc/os-release | grep "^PRETTY_NAME="
}

# Print system uptime
print_uptime() {
    echo "System Uptime:"
    uptime -p
}

# Print top 5 largest files/directories in current directory
print_disk_usage() {
    echo "Top 5 Largest Files/Directories:"
    du -sh ./* 2>/dev/null | sort -hr | head -n 5
}

# Print memory usage
print_memory_usage() {
    echo "Memory Usage:"
    free -h
}

# Print top 5 CPU-consuming processes
print_top_cpu_processes() {
    echo "Top 5 CPU-Consuming Processes:"
    ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -n 6
}

# Main function
main() {
    echo "=================================="
    echo "      SYSTEM INFORMATION"
    echo "=================================="

    echo
    echo "----- Hostname & OS Info -----"
    print_system_info

    echo
    echo "----- Uptime -----"
    print_uptime

    echo
    echo "----- Disk Usage -----"
    print_disk_usage

    echo
    echo "----- Memory Usage -----"
    print_memory_usage

    echo
    echo "----- Top CPU Processes -----"
    print_top_cpu_processes
}

main
```

## Usage

```bash
chmod +x system_info.sh
./system_info.sh
```

## Features

* Uses functions for modularity
* Stops on errors with `set -e`
* Prevents undefined variables with `set -u`
* Handles pipeline failures with `set -o pipefail`
* Provides a quick system health overview

```
```


<img width="1858" height="994" alt="image" src="https://github.com/user-attachments/assets/f6e81df8-1fad-47f2-91fe-9c031b8727fe" />

<img width="1858" height="994" alt="Screenshot from 2026-06-08 10-15-03" src="https://github.com/user-attachments/assets/dc28b356-7e33-4809-bf70-4e89d94652dd" />





