Day 21 – Shell Scripting Cheat Sheet: Build Your Own Reference Guide

####Task 1: Basics #####

Shebang (#!/bin/bash) — what it does and why it matters

Shebang (#!/bin/bash) is the first line of a script that specifies the interpreter to use for executing the script. In this case, /bin/bash tells the operating system to run the script using the Bash shell. This ensures the script is executed with the correct shell and behaves consistently.

Running a script — chmod +x, ./script.sh, bash script.sh

chmod +x script.sh adds execute permission to the script, allowing it to be run directly. A script can be executed using ./script.sh, which requires execute permission and uses the interpreter specified in the shebang. Alternatively, it can be run using bash script.sh, which explicitly invokes the Bash interpreter and does not require execute permission on the script file.`

Comments — single line (#) and inline

# is used to create a single-line comment in a shell script. Comments are ignored by the shell during execution and are used to explain the code or add notes for readability.


Variables — declaring, using, and quoting ($VAR, "$VAR", '$VAR')

VAR=value declares a variable, $VAR accesses its value, "$VAR" expands the value while preserving spaces, and '$VAR' treats the text literally without expanding the variable.

Reading user input — read
The read command is used to take input from the user and store it in a variable. The -p option allows you to display a prompt message before reading the input.

Command-line arguments — $0, $1, $#, $@, $?
$0 contains the script name, $1, $2, etc. contain positional arguments, $# gives the total number of arguments, $@ represents all arguments, and $? stores the exit status of the last command executed.

### Task 2: Operators and Conditionals ####

Document with examples:
String comparisons — =, !=, -z, -n

# String Comparisons in Bash

## 1. Equal To (`=`)

Used to check whether two strings are equal.

```bash
x="hello"

if [ "$x" = "hello" ]; then
    echo "Strings are equal"
fi
```

Output:

```text
Strings are equal
```

---

## 2. Not Equal To (`!=`)

Used to check whether two strings are different.

```bash
x="hello"

if [ "$x" != "world" ]; then
    echo "Strings are not equal"
fi
```

Output:

```text
Strings are not equal
```

---

## 3. String is Empty (`-z`)

Returns true if the string length is zero.

```bash
x=""

if [ -z "$x" ]; then
    echo "String is empty"
fi
```

Output:

```text
String is empty
```

---

## 4. String is Not Empty (`-n`)

Returns true if the string length is greater than zero.

```bash
x="hello"

if [ -n "$x" ]; then
    echo "String is not empty"
fi
```

Output:

```text
String is not empty
```

---

* `=` : Checks if two strings are equal.
* `!=` : Checks if two strings are not equal.
* `-z` : Checks if a string is empty.
* `-n` : Checks if a string is not empty.

Integer comparisons — -eq, -ne, -lt, -gt, -le, -ge

# Integer Comparisons in Bash

Integer comparison operators are used inside test conditions (`[ ]`) to compare numeric values.

## 1. Equal To (`-eq`)

Checks if two integers are equal.

```bash
x=10

if [ "$x" -eq 10 ]; then
    echo "x is equal to 10"
fi
```

---

## 2. Not Equal To (`-ne`)

Checks if two integers are not equal.

```bash
x=10

if [ "$x" -ne 5 ]; then
    echo "x is not equal to 5"
fi
```

---

## 3. Less Than (`-lt`)

Checks if the left integer is less than the right integer.

```bash
x=5

if [ "$x" -lt 10 ]; then
    echo "x is less than 10"
fi
```

---

## 4. Greater Than (`-gt`)

Checks if the left integer is greater than the right integer.

```bash
x=15

if [ "$x" -gt 10 ]; then
    echo "x is greater than 10"
fi
```

---

## 5. Less Than or Equal To (`-le`)

Checks if the left integer is less than or equal to the right integer.

```bash
x=10

if [ "$x" -le 10 ]; then
    echo "x is less than or equal to 10"
fi
```

---

## 6. Greater Than or Equal To (`-ge`)

Checks if the left integer is greater than or equal to the right integer.

```bash
x=10

if [ "$x" -ge 5 ]; then
    echo "x is greater than or equal to 5"
fi
```

---


| Operator | Meaning                  |
| -------- | ------------------------ |
| `-eq`    | Equal to                 |
| `-ne`    | Not equal to             |
| `-lt`    | Less than                |
| `-gt`    | Greater than             |
| `-le`    | Less than or equal to    |
| `-ge`    | Greater than or equal to |

Example:

```bash
a=10
b=20

if [ "$a" -lt "$b" ]; then
    echo "a is less than b"
fi
```


File test operators — -f, -d, -e, -r, -w, -x, -s

# File Test Operators in Bash

File test operators are used inside `[ ]` to check file properties before performing operations.

## 1. File Exists and Is a Regular File (`-f`)

Checks whether a file exists and is a regular file.

```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists and is a regular file"
fi
```

---

## 2. Directory Exists (`-d`)

Checks whether a directory exists.

```bash
if [ -d "logs" ]; then
    echo "logs directory exists"
fi
```

---

## 3. Exists (`-e`)

Checks whether a file or directory exists.

```bash
if [ -e "file.txt" ]; then
    echo "file.txt exists"
fi
```

---

## 4. Readable (`-r`)

Checks whether the file is readable by the current user.

```bash
if [ -r "file.txt" ]; then
    echo "file.txt is readable"
fi
```

---

## 5. Writable (`-w`)

Checks whether the file is writable by the current user.

```bash
if [ -w "file.txt" ]; then
    echo "file.txt is writable"
fi
```

---

## 6. Executable (`-x`)

Checks whether the file is executable by the current user.

```bash
if [ -x "script.sh" ]; then
    echo "script.sh is executable"
fi
```

---

## 7. Not Empty (`-s`)

Checks whether the file exists and has a size greater than zero.

```bash
if [ -s "file.txt" ]; then
    echo "file.txt is not empty"
fi
```

---


| Operator | Meaning                           |
| -------- | --------------------------------- |
| `-f`     | File exists and is a regular file |
| `-d`     | Directory exists                  |
| `-e`     | File or directory exists          |
| `-r`     | File is readable                  |
| `-w`     | File is writable                  |
| `-x`     | File is executable                |
| `-s`     | File exists and is not empty      |

Example:

```bash
if [ -f "$FILE" ] && [ -r "$FILE" ]; then
    echo "File exists and is readable"
fi
```
if, elif, else syntax

# if, elif, else Syntax in Bash

The `if`, `elif`, and `else` statements are used to execute different blocks of code based on conditions.

Syntax:

```bash
if [ condition ]; then
    # commands if condition is true
elif [ condition ]; then
    # commands if second condition is true
else
    # commands if no condition is true
fi
```

Example:

```bash
X=5

if [ "$X" -eq 5 ]; then
    echo "X is 5"
elif [ "$X" -eq 4 ]; then
    echo "X is 4"
else
    echo "X is neither 5 nor 4"
fi
```

Output:

```text
X is 5
```

# Logical Operators in Bash

Logical operators are used to combine or negate conditions in Bash scripts.

## 1. AND Operator (`&&`)

The second command runs only if the first command succeeds.

```bash
[ "$X" -eq 5 ] && echo "X is 5"
```

Example:

```bash
if [ "$X" -eq 5 ] && [ "$Y" -eq 10 ]; then
    echo "Both conditions are true"
fi
```

---

## 2. OR Operator (`||`)

The second command runs only if the first command fails.

```bash
[ "$X" -eq 5 ] || echo "X is not 5"
```

Example:

```bash
if [ "$X" -eq 5 ] || [ "$Y" -eq 10 ]; then
    echo "At least one condition is true"
fi
```

---

## 3. NOT Operator (`!`)

Negates a condition.

```bash
if ! [ "$X" -eq 5 ]; then
    echo "X is not 5"
fi
```

You can also write:

```bash
if [ "$X" -ne 5 ]; then
    echo "X is not 5"
fi
```

---



| Operator | Meaning                                    |   |                                                  |
| -------- | ------------------------------------------ | - | ------------------------------------------------ |
| `&&`     | Logical AND – both conditions must be true |   |                                                  |
| `        |                                            | ` | Logical OR – at least one condition must be true |
| `!`      | Logical NOT – reverses the condition       |   |                                                  |

Example:

```bash
X=5
Y=10

if [ "$X" -eq 5 ] && [ "$Y" -eq 10 ]; then
    echo "Both conditions are true"
fi
```

# Case Statements in Bash (`case ... esac`)

The `case` statement is used to compare a variable against multiple patterns and execute the matching block of code. It is often cleaner than using many `if-elif-else` statements.

## Syntax

```bash
case "$variable" in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        commands
        ;;
esac
```

* `case` starts the statement.
* `)` follows each pattern.
* `;;` ends a pattern block.
* `*` is the default case (similar to `else`).
* `esac` closes the case statement (`case` spelled backward).

---

## Example

```bash
DAY="Monday"

case "$DAY" in
    Monday)
        echo "Start of the work week"
        ;;
    Friday)
        echo "Almost weekend"
        ;;
    Saturday|Sunday)
        echo "Weekend"
        ;;
    *)
        echo "Regular day"
        ;;
esac
```

Output:

```text
Start of the work week
```

---

## Example: User Input

```bash
read -p "Enter y or n: " ANSWER

case "$ANSWER" in
    y|Y)
        echo "You selected Yes"
        ;;
    n|N)
        echo "You selected No"
        ;;
    *)
        echo "Invalid choice"
        ;;
esac
```


> The `case ... esac` statement is used for multi-way branching in Bash. It compares a variable against multiple patterns and executes the matching block. It is often more readable than multiple `if-elif-else` conditions.


# Task 3: Loops in Bash

## 1. for Loop (List-Based)

A list-based `for` loop iterates over a list of values.

```bash
for fruit in apple banana orange
do
    echo "$fruit"
done
```

Output:

```text
apple
banana
orange
```

---

## 2. for Loop (C-Style)

A C-style `for` loop uses initialization, condition, and increment expressions.

```bash
for ((i=1; i<=5; i++))
do
    echo "$i"
done
```

Output:

```text
1
2
3
4
5
```

---

## 3. while Loop

A `while` loop executes as long as the condition is true.

```bash
count=1

while [ "$count" -le 5 ]
do
    echo "$count"
    ((count++))
done
```

Output:

```text
1
2
3
4
5
```

---

## 4. until Loop

An `until` loop executes until the condition becomes true.

```bash
count=1

until [ "$count" -gt 5 ]
do
    echo "$count"
    ((count++))
done
```

Output:

```text
1
2
3
4
5
```

---

## 5. break Statement

`break` exits the loop immediately.

```bash
for i in 1 2 3 4 5
do
    if [ "$i" -eq 3 ]; then
        break
    fi
    echo "$i"
done
```

Output:

```text
1
2
```

---

## 6. continue Statement

`continue` skips the current iteration and moves to the next one.

```bash
for i in 1 2 3 4 5
do
    if [ "$i" -eq 3 ]; then
        continue
    fi
    echo "$i"
done
```

Output:

```text
1
2
4
5
```

---

## 7. Looping Over Files

Process all `.log` files in the current directory.

```bash
for file in *.log
do
    echo "Processing $file"
done
```

Example Output:

```text
Processing app.log
Processing error.log
Processing system.log
```

---

## 8. Looping Over Command Output

Read command output line by line using `while read`.

```bash
cat users.txt | while read line
do
    echo "User: $line"
done
```

Better practice:

```bash
while read -r line
do
    echo "User: $line"
done < users.txt
```

Example Output:

```text
User: alice
User: bob
User: charlie
```

---


* `for` loop: Iterates over a list or range of values.
* `for (( ))`: C-style loop with initialization, condition, and increment.
* `while` loop: Runs while a condition is true.
* `until` loop: Runs until a condition becomes true.
* `break`: Exits a loop immediately.
* `continue`: Skips the current iteration.
* `for file in *.log`: Loops through matching files.
* `while read line`: Processes input line by line.


  # Task 4: Functions in Bash

## 1. Defining a Function

Functions are reusable blocks of code that perform a specific task.

Syntax:

```bash
function_name() {
    commands
}
```

Example:

```bash
greet() {
    echo "Hello, World!"
}
```

---

## 2. Calling a Function

Call a function by using its name.

```bash
greet() {
    echo "Hello, World!"
}

greet
```

Output:

```text
Hello, World!
```

---

## 3. Passing Arguments to Functions

Functions can accept arguments just like scripts.

Inside a function:

* `$1` → First argument
* `$2` → Second argument
* `$#` → Number of arguments
* `$@` → All arguments

Example:

```bash
greet() {
    echo "Hello, $1"
}

greet "Sufiyan"
```

Output:

```text
Hello, Sufiyan
```

Example with multiple arguments:

```bash
show_info() {
    echo "Name: $1"
    echo "Age: $2"
}

show_info "Sufiyan" "25"
```

Output:

```text
Name: Sufiyan
Age: 25
```

---

## 4. Return Values: `return` vs `echo`

### Using `return`

`return` is used to return an exit status from a function.

```bash
check_status() {
    return 0
}

check_status
echo $?
```

Output:

```text
0
```

Important:

* `return` can only return integers from 0 to 255.
* Commonly used to indicate success or failure.

Example:

```bash
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0
    else
        return 1
    fi
}

is_even 4
echo $?
```

Output:

```text
0
```

---

### Using `echo`

`echo` is commonly used to return actual data from a function.

```bash
get_name() {
    echo "Sufiyan"
}

name=$(get_name)
echo "$name"
```

Output:

```text
Sufiyan
```

---

## 5. Local Variables

Variables declared with `local` are only available inside the function.

Example:

```bash
greet() {
    local name="Sufiyan"
    echo "Hello, $name"
}

greet
echo "$name"
```

Output:

```text
Hello, Sufiyan
```

The second `echo` prints nothing because `name` is local to the function.

---

## Complete Example

```bash
#!/bin/bash

greet_user() {
    local name="$1"
    echo "Hello, $name"
}

result=$(greet_user "Sufiyan")
echo "$result"
```

Output:

```text
Hello, Sufiyan
```

---


* Functions are reusable blocks of code.
* Define a function using `function_name() { ... }`.
* Call a function by writing its name.
* Use `$1`, `$2`, etc. to access function arguments.
* Use `return` for exit status (0–255).
* Use `echo` to return actual data.
* Use `local` to create variables that exist only within the function.

# Task 5: Text Processing Commands

## 1. grep

Search for patterns in files.

### Useful Flags

| Flag | Description                            |
| ---- | -------------------------------------- |
| `-i` | Ignore case                            |
| `-r` | Recursive search                       |
| `-c` | Count matching lines                   |
| `-n` | Show line numbers                      |
| `-v` | Invert match (show non-matching lines) |
| `-E` | Extended regular expressions           |

### Examples

```bash
grep "error" app.log
```

```bash
grep -i "error" app.log
```

```bash
grep -r "ERROR" /var/log/
```

```bash
grep -c "ERROR" app.log
```

```bash
grep -n "ERROR" app.log
```

```bash
grep -v "INFO" app.log
```

```bash
grep -E "error|warning" app.log
```

---

## 2. awk

Powerful text-processing tool for column-based data.

### Print Columns

```bash
awk '{print $1}' file.txt
```

Print first and third columns:

```bash
awk '{print $1,$3}' file.txt
```

### Field Separator

```bash
awk -F: '{print $1}' /etc/passwd
```

### Pattern Matching

```bash
awk '/ERROR/ {print $0}' app.log
```

### BEGIN and END

```bash
awk '
BEGIN {print "Start"}
{print $1}
END {print "End"}
' file.txt
```

---

## 3. sed

Stream editor for modifying text.

### Substitute Text

```bash
sed 's/error/warning/' file.txt
```

Replace all occurrences:

```bash
sed 's/error/warning/g' file.txt
```

### Delete Lines

Delete line 3:

```bash
sed '3d' file.txt
```

Delete blank lines:

```bash
sed '/^$/d' file.txt
```

### In-place Edit

```bash
sed -i 's/error/warning/g' file.txt
```

---

## 4. cut

Extract specific fields from text.

### By Delimiter

```bash
cut -d: -f1 /etc/passwd
```

Output:

```text
root
ubuntu
mysql
```

### Multiple Fields

```bash
cut -d: -f1,3 /etc/passwd
```

---

## 5. sort

Sort lines of text.

### Alphabetical Sort

```bash
sort names.txt
```

### Numerical Sort

```bash
sort -n numbers.txt
```

### Reverse Sort

```bash
sort -r names.txt
```

### Unique Sort

```bash
sort -u names.txt
```

---

## 6. uniq

Remove adjacent duplicate lines.

### Remove Duplicates

```bash
uniq names.txt
```

### Count Occurrences

```bash
uniq -c names.txt
```

Common usage:

```bash
sort names.txt | uniq -c
```

---

## 7. tr

Translate or delete characters.

### Convert Lowercase to Uppercase

```bash
echo "hello" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO
```

### Delete Characters

```bash
echo "hello123" | tr -d '0-9'
```

Output:

```text
hello
```

---

## 8. wc

Count lines, words, and characters.

### Line Count

```bash
wc -l file.txt
```

### Word Count

```bash
wc -w file.txt
```

### Character Count

```bash
wc -m file.txt
```

### All Counts

```bash
wc file.txt
```

---

## 9. head

Display first N lines.

### First 10 Lines

```bash
head file.txt
```

### First 5 Lines

```bash
head -n 5 file.txt
```

---

## 10. tail

Display last N lines.

### Last 10 Lines

```bash
tail file.txt
```

### Last 5 Lines

```bash
tail -n 5 file.txt
```

### Follow Log File

```bash
tail -f app.log
```

Shows new lines as they are written to the file.

---

# Interview Cheat Sheet

```text
grep  -> Search text
awk   -> Process columns and patterns
sed   -> Edit text streams
cut   -> Extract fields
sort  -> Sort lines
uniq  -> Remove duplicates
tr    -> Translate/delete characters
wc    -> Count lines, words, chars
head  -> First N lines
tail  -> Last N lines / follow logs
```

# Task 6: Useful Patterns and One-Liners

These one-liners are commonly used by Linux administrators, DevOps engineers, and SREs for troubleshooting, monitoring, and automation.

---

## 1. Find and Delete Files Older Than N Days

Delete `.log` files older than 30 days:

```bash
find /var/log -name "*.log" -type f -mtime +30 -delete
```

Preview files before deleting:

```bash
find /var/log -name "*.log" -type f -mtime +30
```

Explanation:

* `-name "*.log"` → Match log files
* `-type f` → Files only
* `-mtime +30` → Older than 30 days
* `-delete` → Remove files

---

## 2. Count Lines in All .log Files

Count lines per file:

```bash
wc -l *.log
```

Total lines across all logs:

```bash
cat *.log | wc -l
```

Using find:

```bash
find . -name "*.log" -exec wc -l {} +
```

---

## 3. Replace a String Across Multiple Files

Replace "localhost" with "db-server":

```bash
find . -type f -name "*.conf" -exec sed -i 's/localhost/db-server/g' {} +
```

Preview changes first:

```bash
grep -rn "localhost" .
```

---

## 4. Check if a Service is Running

Check nginx:

```bash
systemctl is-active nginx
```

Detailed status:

```bash
systemctl status nginx
```

One-line check:

```bash
systemctl is-active --quiet nginx && echo "Running" || echo "Stopped"
```

---

## 5. Monitor Disk Usage with Alerts

Alert when usage exceeds 80%:

```bash
df -h / | awk 'NR==2 {gsub("%","",$5); if($5>80) print "ALERT: Disk usage above 80%"}'
```

More detailed:

```bash
df -h | awk '$5+0 > 80 {print "Warning:", $0}'
```

---

## 6. Parse CSV from Command Line

CSV file:

```text
name,age,city
Alice,25,Mumbai
Bob,30,Delhi
```

Print names:

```bash
cut -d',' -f1 users.csv
```

Using awk:

```bash
awk -F',' '{print $1}' users.csv
```

Print name and city:

```bash
awk -F',' '{print $1,$3}' users.csv
```

---

## 7. Parse JSON from Command Line

JSON file:

```json
{
  "name":"Sufiyan",
  "role":"DevOps"
}
```

Extract values using jq:

```bash
jq '.name' user.json
```

```bash
jq '.role' user.json
```

Pretty print JSON:

```bash
jq '.' user.json
```

---

## 8. Tail a Log and Filter Errors in Real Time

Show only error messages:

```bash
tail -f app.log | grep ERROR
```

Case-insensitive:

```bash
tail -f app.log | grep -i error
```

Show errors and warnings:

```bash
tail -f app.log | grep -E "ERROR|WARN"
```

---

## 9. Find Top 10 Largest Files

```bash
find . -type f -exec du -h {} + | sort -hr | head -10
```

Useful when troubleshooting disk usage.

---

## 10. Find Top 5 CPU-Consuming Processes

```bash
ps aux --sort=-%cpu | head -6
```

Explanation:

* `--sort=-%cpu` → Highest CPU first
* `head -6` → Header + top 5 processes

---

## 11. Find Top 5 Memory-Consuming Processes

```bash
ps aux --sort=-%mem | head -6
```

Useful during memory troubleshooting.

---

## 12. Check Listening Ports

```bash
ss -tulpn
```

Filter for port 80:

```bash
ss -tulpn | grep :80
```

---

## 13. Search Logs for Errors

```bash
grep -ri "error" /var/log
```

Count errors:

```bash
grep -ric "error" /var/log
```

---

## 14. Find Failed Login Attempts

```bash
grep "Failed password" /var/log/auth.log
```

Count failed logins:

```bash
grep -c "Failed password" /var/log/auth.log
```

---

## 15. Most Common IP Addresses in Access Logs

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

Example Output:

```text
120 192.168.1.10
87 10.0.0.5
45 172.16.1.20
```


```bash
# Check service status
systemctl is-active nginx

# Find old files
find . -mtime +30

# Monitor logs
tail -f app.log

# Top CPU processes
ps aux --sort=-%cpu | head

# Disk usage
df -h

# Listening ports
ss -tulpn

# Search logs
grep -ri error /var/log

# Top IPs in logs
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```
# Task 7: Error Handling and Debugging in Bash

Proper error handling makes scripts more reliable, easier to debug, and safer to run in production.

---

## 1. Exit Codes (`$?`, `exit 0`, `exit 1`)

Every command returns an exit status.

* `0` → Success
* Non-zero → Error or failure

### Check Exit Status

```bash
ls /tmp
echo $?
```

Output:

```text
0
```

Example of failure:

```bash
ls /nonexistent
echo $?
```

Output:

```text
2
```

### Using exit

Successful script:

```bash
echo "Backup completed"
exit 0
```

Failed script:

```bash
echo "Backup failed"
exit 1
```

---

## 2. set -e (Exit on Error)

Makes the script stop immediately if a command fails.

```bash
#!/bin/bash

set -e

echo "Starting"

ls /nonexistent

echo "This line will never execute"
```

Output:

```text
Starting
ls: cannot access '/nonexistent': No such file or directory
```

Without `set -e`, the script would continue running.

---

## 3. set -u (Treat Unset Variables as Errors)

Prevents the use of undefined variables.

```bash
#!/bin/bash

set -u

echo "$NAME"
```

Output:

```text
bash: NAME: unbound variable
```

This helps catch typos in variable names.

Example:

```bash
USER_NAME="sufiyan"

echo "$USER_NMAE"
```

Without `set -u`, Bash prints an empty value.

With `set -u`, Bash reports an error.

---

## 4. set -o pipefail (Catch Errors in Pipelines)

Normally, a pipeline returns the exit status of the last command.

Example:

```bash
grep "ERROR" missing.log | wc -l
echo $?
```

Output:

```text
0
```

Even though `grep` failed.

Enable pipefail:

```bash
set -o pipefail

grep "ERROR" missing.log | wc -l
echo $?
```

Output:

```text
2
```

Now the pipeline correctly reports failure.

---

## 5. set -x (Debug Mode)

Prints each command before executing it.

```bash
#!/bin/bash

set -x

name="Sufiyan"
echo "$name"
```

Output:

```text
+ name=Sufiyan
+ echo Sufiyan
Sufiyan
```

Useful when troubleshooting scripts.

Disable debugging:

```bash
set +x
```

---

## 6. Common Production Pattern

Many scripts start with:

```bash
set -euo pipefail
```

Meaning:

```bash
set -e          # Exit on error
set -u          # Error on unset variables
set -o pipefail # Catch pipeline failures
```

This is considered a Bash best practice.

---

## 7. Trap (Cleanup on Exit)

`trap` executes commands when a signal or event occurs.

### Basic Cleanup Example

```bash
cleanup() {
    echo "Cleaning up temporary files"
    rm -f /tmp/mytempfile
}

trap cleanup EXIT
```

When the script exits, the cleanup function runs automatically.

---

## 8. Temporary File Example

```bash
#!/bin/bash

set -euo pipefail

TEMP_FILE=$(mktemp)

cleanup() {
    rm -f "$TEMP_FILE"
}

trap cleanup EXIT

echo "Working..."
echo "data" > "$TEMP_FILE"
```

Even if the script fails, the temporary file is removed.

---

## 9. Trap Interrupt Signal (Ctrl+C)

```bash
trap 'echo "Interrupted"; exit 1' INT
```

Pressing Ctrl+C produces:

```text
Interrupted
```

and exits cleanly.

---

## Complete Example

```bash
#!/bin/bash

set -euo pipefail

cleanup() {
    echo "Cleanup completed"
}

trap cleanup EXIT

FILE="data.txt"

if [ ! -f "$FILE" ]; then
    echo "File not found"
    exit 1
fi

echo "Processing file..."
```

This script:

* Stops on errors
* Detects unset variables
* Detects pipeline failures
* Cleans up automatically
* Uses meaningful exit codes

---



| Command             | Purpose                       |
| ------------------- | ----------------------------- |
| `$?`                | Exit status of last command   |
| `exit 0`            | Success                       |
| `exit 1`            | Failure                       |
| `set -e`            | Exit on error                 |
| `set -u`            | Error on unset variables      |
| `set -o pipefail`   | Catch pipeline failures       |
| `set -x`            | Debug mode                    |
| `trap cleanup EXIT` | Run cleanup when script exits |

### Most Common Production Header

```bash
#!/bin/bash

set -euo pipefail
```

This is the header you'll see in many professional Bash scripts.

Task 8: Bonus — Quick Reference Table

# Bash Scripting Cheat Sheet

## Quick Reference Table

| Topic | Key Syntax | Example |
|--------|------------|---------|
| Shebang | `#!/bin/bash` | `#!/bin/bash` |
| Variable | `VAR="value"` | `NAME="DevOps"` |
| Read Input | `read variable` | `read -p "Name: " NAME` |
| Argument | `$1`, `$2` | `./script.sh arg1 arg2` |
| Script Name | `$0` | `echo "$0"` |
| Argument Count | `$#` | `echo "$#"` |
| All Arguments | `$@` | `echo "$@"` |
| Exit Status | `$?` | `echo "$?"` |
| String Compare | `[ "$a" = "$b" ]` | `if [ "$x" = "hello" ]; then` |
| Integer Compare | `-eq`, `-ne`, `-lt`, `-gt` | `if [ "$x" -eq 5 ]; then` |
| File Check | `-f`, `-d`, `-e` | `if [ -f file.txt ]; then` |
| If Statement | `if [ condition ]; then` | `if [ -f file.txt ]; then` |
| Case Statement | `case ... esac` | `case "$opt" in` |
| For Loop | `for i in list; do` | `for i in 1 2 3; do` |
| While Loop | `while [ condition ]; do` | `while [ "$x" -lt 5 ]; do` |
| Until Loop | `until [ condition ]; do` | `until [ "$x" -gt 5 ]; do` |
| Function | `name() { ... }` | `greet() { echo "Hi"; }` |
| Return Code | `return 0` | `return 1` |
| Local Variable | `local var=value` | `local name="DevOps"` |
| Grep | `grep pattern file` | `grep -i "error" log.txt` |
| Awk | `awk '{print $1}' file` | `awk -F: '{print $1}' /etc/passwd` |
| Sed | `sed 's/old/new/g' file` | `sed -i 's/foo/bar/g' config.txt` |
| Cut | `cut -d: -f1 file` | `cut -d, -f2 users.csv` |
| Sort | `sort file` | `sort -nr numbers.txt` |
| Uniq | `uniq` | `sort file | uniq -c` |
| Tr | `tr 'a-z' 'A-Z'` | `echo "hi" \| tr 'a-z' 'A-Z'` |
| Word Count | `wc -l` | `wc -l app.log` |
| Head | `head -n 5 file` | `head -n 10 log.txt` |
| Tail | `tail -f file` | `tail -f app.log` |
| Exit on Error | `set -e` | `set -e` |
| Unset Variable Error | `set -u` | `set -u` |
| Pipe Failure Detection | `set -o pipefail` | `set -o pipefail` |
| Debug Mode | `set -x` | `set -x` |
| Trap | `trap 'cleanup' EXIT` | `trap cleanup EXIT` |

---

## Recommended Script Header

```bash
#!/bin/bash

set -euo pipefail
```

This enables:

- `-e` → Exit on error
- `-u` → Error on unset variables
- `pipefail` → Catch pipeline failures

