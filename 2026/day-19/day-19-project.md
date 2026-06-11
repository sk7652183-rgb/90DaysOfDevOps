Day 19 – Shell Scripting Project: Log Rotation, Backup & Crontab

# Log Rotation Script

## Description

This script performs basic log rotation tasks:

* Accepts a log directory as an argument.
* Compresses `.log` files older than 7 days using `gzip`.
* Deletes compressed `.gz` files older than 30 days.
* Displays the number of files compressed and deleted.
* Exits with an error if the specified directory does not exist.

## Script

```bash
#!/bin/bash

set -euo pipefail

# Check if directory argument is provided

if [ $# -ne 1 ]; then
    echo "Usage: $0 <log_directory>"
    exit 1
fi

LOG_DIR="$1"

# Check if the directory exists

if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory '$LOG_DIR' does not exist."
    exit 1
fi

# Count the log files older than 7 days

compressed_count=$(find "$LOG_DIR" -type f -name "*.log" -mtime +7 | wc -l)

# Compress .log files older than 7 days

find "$LOG_DIR" -type f -name "*.log" -mtime +7 -exec gzip {} \;

# Count old .gz files older than 30 days

deleted_count=$(find "$LOG_DIR" -type f -name "*.gz" -mtime +30 | wc -l)

# Delete .gz files older than 30 days

find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -delete

echo "Compressed files: $compressed_count"
echo "Deleted files: $deleted_count"
```

## Usage

```bash
chmod +x log_rotate.sh
./log_rotate.sh /path/to/log_directory
```

## Example Output

```text
Compressed files: 5
Deleted files: 2
```



<img width="1856" height="229" alt="image" src="https://github.com/user-attachments/assets/969b3430-3ef8-4bf5-8cdc-eaa09534bccf" />

<img width="1860" height="996" alt="image" src="https://github.com/user-attachments/assets/9140557f-c7c3-48da-8a37-81fe17a0802b" />

****Task 2: Server Backup Script************

# Backup Script

## Description

This script creates a compressed backup of a source directory and stores it in a specified backup destination.

### Features

* Takes a source directory and backup destination as arguments.
* Creates a timestamped `.tar.gz` archive.
* Verifies that the archive was created successfully.
* Displays the archive name and size.
* Deletes backup files older than 14 days.
* Exits with an error if the source or destination directory does not exist.
* Uses `set -euo pipefail` for safer script execution.

## Script

```bash
#!/bin/bash

set -euo pipefail

# Check arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_directory> <backup_destination>"
    exit 1
fi

SOURCE_DIR="$1"
BACKUP_DIR="$2"

# Verify source directory exists
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory '$SOURCE_DIR' does not exist."
    exit 1
fi

# Verify backup destination exists
if [ ! -d "$BACKUP_DIR" ]; then
    echo "Error: Backup destination '$BACKUP_DIR' does not exist."
    exit 1
fi

# Create timestamped archive name
TIMESTAMP=$(date +%F)
ARCHIVE_NAME="backup-${TIMESTAMP}.tar.gz"
ARCHIVE_PATH="${BACKUP_DIR}/${ARCHIVE_NAME}"

# Create archive
tar -czf "$ARCHIVE_PATH" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

# Verify archive was created
if [ ! -f "$ARCHIVE_PATH" ]; then
    echo "Error: Failed to create archive."
    exit 1
fi

# Get archive size
ARCHIVE_SIZE=$(du -h "$ARCHIVE_PATH" | cut -f1)

echo "Backup created successfully."
echo "Archive: $ARCHIVE_NAME"
echo "Size: $ARCHIVE_SIZE"

# Delete backups older than 14 days
find "$BACKUP_DIR" -type f -name "backup-*.tar.gz" -mtime +14 -delete

echo "Old backups older than 14 days have been removed."
```

## Usage

```bash
chmod +x backup.sh
./backup.sh /path/to/source_directory /path/to/backup_directory
```

## Example

```bash
./backup.sh /home/ubuntu/projects /home/ubuntu/backups
```

## Example Output

```text
Backup created successfully.
Archive: backup-2026-06-10.tar.gz
Size: 15M
Old backups older than 14 days have been removed.
```


<img width="1859" height="491" alt="image" src="https://github.com/user-attachments/assets/e7759938-83d9-48a3-b0da-82cbde18a440" />

<img width="1860" height="1050" alt="image" src="https://github.com/user-attachments/assets/8c9ce14d-9aa9-49a3-9de3-587e0cfe7da8" />

***Task 3: Crontab*****

crontab -l — what's currently scheduled?

Nothing is scheduled currently 

<img width="1867" height="75" alt="image" src="https://github.com/user-attachments/assets/a09adc99-0abe-49ed-afb2-f2bb6d5aa6ed" />

All scripts have been scheduled according to the given conditions.

<img width="1859" height="777" alt="image" src="https://github.com/user-attachments/assets/0a669e69-6778-44a2-8ad5-e9bd27dd2f67" />


******Task 4: Combine — Scheduled Maintenance Script****************************

# maintenance.sh

```bash
#!/bin/bash

set -euo pipefail

LOG_FILE="/var/log/maintenance.log"

log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" >> "$LOG_FILE"
}

main() {
    log_message "Maintenance job started."

    log_message "Running log rotation..."
    /home/sufiyan/DevOps/Scripts/log_rotate.sh /var/log >> "$LOG_FILE" 2>&1

    log_message "Running backup..."
    /home/sufiyan/DevOps/Scripts/backup.sh \
        /home/sufiyan/DevOps/Scripts \
        /home/sufiyan/DevOps/backups >> "$LOG_FILE" 2>&1

    log_message "Maintenance job completed."
}

main
```

## Make Script Executable

```bash
chmod +x /home/sufiyan/DevOps/Scripts/maintenance.sh
```

## Cron Entry

Run the maintenance script every day at 1:00 AM:

```cron
0 1 * * * /home/sufiyan/DevOps/Scripts/maintenance.sh
```

## Example Log Output

```text
[2026-06-10 23:58:39] Maintenance job started.
[2026-06-10 23:58:39] Running log rotation.....
Compressed files: 0
Deleted files: 0

[2026-06-11 20:01:40] Maintenance job started.
[2026-06-11 20:01:40] Running log rotation.....
Compressed files: 0
Deleted files: 0

[2026-06-11 20:01:40] Running backup.....
Backup created successfully.
Archive: backup-2026-06-11_20-01-40.tar.gz
Size: 12K
Old backups older than 14 days have been removed.

[2026-06-11 20:01:40] Maintenance job completed.
```



<img width="1920" height="1038" alt="image" src="https://github.com/user-attachments/assets/5b349704-591d-4875-9a49-52e7e039caf6" />







