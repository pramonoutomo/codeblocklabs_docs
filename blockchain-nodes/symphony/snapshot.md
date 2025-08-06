---
description: Get synced faster and easy.
---

# Snapshot

```bash
sudo apt update
sudo apt-get install snapd lz4 -y
```

## Chain Snapshots

Create new file with "sudo nano download\_symphony.sh"

```
#!/bin/bash

# Config
BACKUP_URL="https://green.codeblocklabs.com/mainnet/symphony/"
TARGET_DIR="$HOME/.symphonyd"
LOG_FILE="$HOME/symphony_backup.log"

# Get today's and yesterday's dates in YYYY-MM-DD format
TODAY=$(date '+%Y-%m-%d')
YESTERDAY=$(date -d 'yesterday' '+%Y-%m-%d')

# Check if backup exists (today or yesterday)
check_backup() {
    local date=$1
    BACKUP_NAME="backup-${date}.tar.lz4"
    
    # Check if file exists on server
    if curl --output /dev/null --silent --head --fail "${BACKUP_URL}${BACKUP_NAME}"; then
        echo "$BACKUP_NAME"
        return 0
    else
        return 1
    fi
}

# Download and extract backup
download_and_extract() {
    local backup_name="$1"
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ✅ Found backup: $backup_name" | tee -a "$LOG_FILE"
    
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ⬇️ Downloading and extracting..." | tee -a "$LOG_FILE"
    mkdir -p "$TARGET_DIR"
    
    # Stream download → lz4 decompress → tar extract
    if curl -sL "${BACKUP_URL}${backup_name}" | lz4 -c -d - | tar -x -C "$TARGET_DIR"; then
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] ✅ Successfully extracted to $TARGET_DIR" | tee -a "$LOG_FILE"
    else
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] ❌ Extraction failed!" | tee -a "$LOG_FILE"
        exit 1
    fi
}

# Main execution
echo "=== Axone Backup Downloader ===" | tee -a "$LOG_FILE"
echo "[$(date '+%Y-%m-%d %H:%M:%S')] 🔍 Checking for today's backup ($TODAY)..." | tee -a "$LOG_FILE"

# Try today's backup first
if BACKUP_NAME=$(check_backup "$TODAY"); then
    download_and_extract "$BACKUP_NAME"
else
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] ⏳ Today's backup not found. Checking yesterday ($YESTERDAY)..." | tee -a "$LOG_FILE"
    if BACKUP_NAME=$(check_backup "$YESTERDAY"); then
        download_and_extract "$BACKUP_NAME"
    else
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] ❌ No backup found for today or yesterday!" | tee -a "$LOG_FILE"
        exit 1
    fi
fi
```

Make it executable

```
chmod +x download_symphony_backup.sh
```

Run the download snapshot

```
./download_symphony_backup.sh
```

After succesfully download snapshot, run the binary by starting the services

```
// Start the nodes on services
sudo systemctl start symphonyd.service && sudo journalctl -u symphonyd.service -f --no-hostname -o cat
```
