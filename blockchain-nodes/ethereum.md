---
description: This guide is only for Ethereum Sepolia - Execution & Consensus Node
icon: server
---

# Ethereum

### System Requirement:

* CPU: 4 Core (minimum), 8 Core (recomended)
* RAM: 16Gb (minimum), 32Gb (recomended)
* Disk: 1Tb (as of this documentation written, it's enough, but it'll increased soon).

Recomended to use vps from: [Contabo VDS](https://lihat.info/contaboVDS), [Hetzner](https://lihat.info/hetzner), [ServaRica](https://lihat.info/servarica)

***

## Installation

### Installing Dependencies:

```
apt -y update && apt -y upgrade
apt-get install coreutils curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
apt dist-upgrade && sudo apt autoremove
```

### Activate Firewall & Open Port:

```
sudo ufw allow 8545/tcp
sudo ufw allow 3500/tcp
sudo ufw allow 4000/tcp
sudo ufw allow 30303/tcp
sudo ufw allow 30303/udp
sudo ufw allow 12000/udp
sudo ufw allow 13000/tcp
sudo ufw allow 22/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

```
# To check status of your firewall
sudo ufw status
```

### Add New Users & Group:

```
sudo adduser --home /home/geth --disabled-password --gecos 'Geth Client' geth
sudo adduser --home /home/beacon --disabled-password --gecos 'Prysm Beacon Client' beacon
sudo groupadd eth
sudo usermod -a -G eth geth
sudo usermod -a -G eth beacon
```

### Generate JWT Secret:

```
sudo mkdir -p /var/lib/secrets
```

```
sudo chgrp -R eth /var/lib/ /var/lib/secrets
```

```
sudo chmod 750 /var/lib/ /var/lib/secrets
```

```
sudo openssl rand -hex 32 | tr -d '\n' | sudo tee /var/lib/secrets/jwt.hex > /dev/null
```

```
sudo chown root:eth /var/lib/secrets/jwt.hex
sudo chmod 640 /var/lib/secrets/jwt.hex
```

### Create Directory for `geth` and `beacon`:

```
sudo -u geth mkdir /home/geth/geth
sudo -u beacon mkdir /home/beacon/beacon
```

### Install Ethereum & geth:

```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.15.11-36b2371c.tar.gz
```

```
tar -xvf geth-linux-amd64-1.15.11-36b2371c.tar.gz
```

```
sudo mv geth-linux-amd64-1.15.11-36b2371c/geth /usr/bin/geth
```

### Create `geth` Service:

```
sudo nano /etc/systemd/system/geth.service
```

```
[Unit]

Description=Geth
After=network-online.target
Wants=network-online.target

[Service]

Type=simple
Restart=always
RestartSec=5s
User=geth
WorkingDirectory=/home/geth
ExecStart=/usr/bin/geth \
  --sepolia \
  --http \
  --http.addr "0.0.0.0" \
  --http.port 8545 \
  --http.api "eth,net,engine,admin" \
  --authrpc.addr "127.0.0.1" --authrpc.port 8551 \
  --http.corsdomain "*" \
  --http.vhosts "*" \
  --datadir /home/geth/geth \
  --authrpc.jwtsecret /var/lib/secrets/jwt.hex

[Install]
WantedBy=multi-user.target
```

#### Start & Enable `geth` Service

```
sudo systemctl daemon-reload
sudo systemctl start geth
sudo systemctl enable geth
```

#### Check `geth` Status:

```
sudo systemctl status geth
```

#### Check `geth` Logs:

```
sudo journalctl -fu geth
```

***

### Create `beacon` Directory & Configure `prysm`:

```
sudo -u beacon mkdir /home/beacon/bin
sudo -u beacon curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output /home/beacon/bin/prysm.sh
sudo -u beacon chmod +x /home/beacon/bin/prysm.sh
```

#### Create `beacon` Service:

```
sudo nano /etc/systemd/system/beacon.service
```

```
[Unit]

Description=Prysm Beacon
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
Restart=always
RestartSec=5s
User=beacon
ExecStart=/home/beacon/bin/prysm.sh beacon-chain \
  --sepolia \
  --http-modules=beacon,config,node,validator \
  --rpc-host=0.0.0.0 --rpc-port=4000 \
  --grpc-gateway-host=0.0.0.0 --grpc-gateway-port=3500 \
  --datadir /home/beacon/beacon \
  --execution-endpoint=http://127.0.0.1:8551 \
  --jwt-secret=/var/lib/secrets/jwt.hex \
  --checkpoint-sync-url=https://checkpoint-sync.sepolia.ethpandaops.io/ \
  --genesis-beacon-api-url=https://checkpoint-sync.sepolia.ethpandaops.io/ \
  --accept-terms-of-use

[Install]
WantedBy=multi-user.targe
```

#### Start & Enable `beacon` Service:

```
sudo systemctl daemon-reload
sudo systemctl start beacon
sudo systemctl enable beacon
```

#### Check `beacon` Status:

```
sudo systemctl status beacon
```

#### Check `beacon` Logs:

```
sudo journalctl -fu beacon
```

**\*wait until both of `geth` and `beacon` are fully synced (could take a few hours or even a day).**

***

### You can check your sync by running this script:

```
nano sync.sh
```

Insert code below

```
#!/bin/bash

echo "=== GETH SYNC STATUS ==="
curl -s -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}' \
     -H "Content-Type: application/json" http://localhost:8545 | jq

echo ""
echo "=== BEACON SYNC STATUS ==="
curl -s http://localhost:3500/eth/v1/node/syncing | jq
```

```
# Change File Access
chmod +x sync.sh
```

To check the sync status, use command below:

```
./sync.sh
```

After synced, your ethereum sepolia Execution & Consensus Node RPC are ready to use.
