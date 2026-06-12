Day 20 – Bash Scripting Challenge: Log Analyzer and Report Generator

## Input Validation

The script expects a log file path as a command-line argument.

### Features

* Verifies that a log file path is provided.
* Displays a usage message if no argument is supplied.
* Checks whether the specified file exists.
* Exits with an error if the file is not found.

### Code

```bash
# Check if a log file path was provided
if [ $# -eq 0 ]; then
    echo "Error: No log file provided."
    echo "Usage: $0 <log_file>"
    exit 1
fi

LOG_FILE="$1"

# Check if the file exists
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

echo "Processing log file: $LOG_FILE"
```

### Example Usage

```bash
./log_analyzer.sh sample.log
```

### Example Output

```text
Processing log file: sample.log
```

### Error Cases

No argument provided:

```text
Error: No log file provided.
Usage: ./log_analyzer.sh <log_file>
```

File does not exist:

```text
Error: File 'missing.log' does not exist.
```


<img width="1863" height="1021" alt="image" src="https://github.com/user-attachments/assets/f7fc1464-1d54-42a3-8826-4d92d073e808" />

******Task 2: Error Count****************

Counted the total number of lines containing the keywords ERROR or FAILED

```bash
grep "ERROR" sample.log
```

<img width="1862" height="1033" alt="image" src="https://github.com/user-attachments/assets/1473dbe5-c36e-4b99-98e4-43414a34bc9b" /> 

### Displayed the total error count on the console.


```bash
grep -E "ERROR|Failed" sample.log | wc -l
```

<img width="1651" height="97" alt="image" src="https://github.com/user-attachments/assets/4242e0a9-bd5e-4c02-b390-8a0227022c3f" />

#### Task 3: Critical Events

Searched for lines containing the keyword CRITICAL

Printed those lines along with their line numbers.

<img width="1860" height="1003" alt="image" src="https://github.com/user-attachments/assets/ab312d4d-8da6-49d4-b223-57801503b162" />


##### Task 4: Top Error Messages

### Top 5 Error Messages

```bash
echo "--- Top 5 Error Messages ---"
grep "ERROR" sample.log | sed 's/^\[[^]]*\] ERROR //' | sort | uniq -c | sort -nr | head -5
```

**Output**

```text
--- Top 5 Error Messages ---
3904 Database connection timeout from 10.0.1.50:3306
3843 Permission denied while accessing /var/www/html
3807 Failed to authenticate user 'admin'
3803 Failed to resolve hostname api.internal.local
```

<img width="1860" height="190" alt="image" src="https://github.com/user-attachments/assets/57dcba44-6b28-4eab-b12b-405f33c30d62" />

### Task 5: Summary Report

# Log Analysis Report Generator

## Description

This script analyzes a log file and generates a report named `log_report_<date>.txt`.

### Features

* Counts the total number of lines in the log file.
* Counts all lines containing `ERROR` or `Failed`.
* Identifies the top 5 most common error messages.
* Lists all `CRITICAL` events with their line numbers.
* Generates a timestamped report file.
* Uses `set -euo pipefail` for safer execution.

## Script

```bash
#!/bin/bash

set -euo pipefail

LOG_FILE="sample.log"
REPORT_FILE="log_report_$(date +%F).txt"

# Collect statistics
total_lines=$(wc -l < "$LOG_FILE")
total_errors=$(grep -E "ERROR|Failed" "$LOG_FILE" | wc -l)

# Generate report
{
    echo "========================================"
    echo "         LOG ANALYSIS REPORT"
    echo "========================================"
    echo
    echo "Date of Analysis : $(date +%F)"
    echo "Log File         : $LOG_FILE"
    echo "Total Lines      : $total_lines"
    echo "Total Errors     : $total_errors"
    echo

    echo "--- Top 5 Error Messages ---"
    echo

    grep "ERROR" "$LOG_FILE" \
        | sed 's/^\[[^]]*\] ERROR //' \
        | sort \
        | uniq -c \
        | sort -nr \
        | head -5

    echo
    echo "--- Critical Events ---"
    echo

    grep -n "CRITICAL" "$LOG_FILE" || echo "No critical events found."

    echo
    echo "========================================"
    echo "           END OF REPORT"
    echo "========================================"

} > "$REPORT_FILE"

echo "Report generated: $REPORT_FILE"
```

## Usage

```bash
chmod +x log_report.sh
./log_report.sh
```

## Example Output

```text
========================================
         LOG ANALYSIS REPORT
========================================

Date of Analysis : 2026-06-12
Log File         : sample.log
Total Lines      : 50000
Total Errors     : 15357

--- Top 5 Error Messages ---

3904 Database connection timeout from 10.0.1.50:3306
3843 Permission denied while accessing /var/www/html
3807 Failed to authenticate user 'admin'
3803 Failed to resolve hostname api.internal.local

--- Critical Events ---

15: [2026-06-12 10:00:01] CRITICAL Database server unreachable for more than 5 minutes
127: [2026-06-12 10:00:05] CRITICAL Disk /dev/sda1 is 100% full
842: [2026-06-12 10:01:10] CRITICAL Kernel panic detected on node-01

========================================
           END OF REPORT
========================================
```


<img width="1843" height="992" alt="image" src="https://github.com/user-attachments/assets/4d21b138-c82c-4658-839a-4acf8a96a8aa" />




