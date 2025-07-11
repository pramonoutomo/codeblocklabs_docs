---
description: Lumera Testnet
---

# Installation

Installation

```
// Install Dependencies
sudo apt-get update
sudo apt-get install curl git jq lz4 build-essential
sudo apt-get upgrade

// Install Go
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.24.1.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)

// Download binaries
mkdir -p $HOME/.axoned/cosmovisor/genesis/bin
wget -O $HOME/.axoned/cosmovisor/genesis/bin/axoned https://green.codeblocklabs.com/mainnet/axone/axoned
chmod +x $HOME/.axoned/cosmovisor/genesis/bin/axoned

// Create application symlinks
ln -s $HOME/.axoned/cosmovisor/genesis $HOME/.axoned/cosmovisor/current -f
sudo ln -s $HOME/.axoned/cosmovisor/current/bin/axoned /usr/local/bin/axoned -f
```

***

## Setting Up Services

```
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0

# Create service
sudo tee /etc/systemd/system/axoned.service > /dev/null << EOF
[Unit]
Description=axone node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.axoned"
Environment="DAEMON_NAME=axoned"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.lumera/cosmovisor/current/bin"

[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable lumera.service

```

***

## Setting Up Nodes

```
# Set node configuration
axoned config chain-id axone-1

# Initialize the node
lumerad init YourNodesName --chain-id axone-1

# Download genesis and addrbook
curl -Ls https://green.codeblocklabs.com/mainnet/axone/genesis.json > $HOME/.axoned/config/genesis.json
curl -Ls https://green.codeblocklabs.com/mainnet/axone/addrbook.json > $HOME/.axoned/config/addrbook.json

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025uaxone\"|" $HOME/.axoned/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.axoned/config/app.toml

# Set custom ports
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:16958\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:16957\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:16960\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:16956\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":16966\"%" $HOME/.axoned/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:16917\"%; s%^address = \":8080\"%address = \":16980\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:16990\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:16991\"%; s%:8545%:16945%; s%:8546%:16946%; s%:6065%:16965%" $HOME/.axoned/config/app.toml

```

## Chain Snapshots

Create new file with "sudo nano download\_axone.sh"

```
#!/bin/bash

# Config
BACKUP_URL="https://green.codeblocklabs.com/mainnet/axone/"
TARGET_DIR="$HOME/.axoned"
LOG_FILE="$HOME/axone_backup.log"

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
chmod +x download_axone_backup.sh
```

Run the download snapshot

```
./download_axone_backup.sh
```

After succesfully download snapshot, run the binary by starting the services

```
// Start the nodes on services
sudo systemctl start axoned.service && sudo journalctl -u axoned.service -f --no-hostname -o cat
```

