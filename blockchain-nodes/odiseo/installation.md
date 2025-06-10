---
description: Lumera Testnet
---

# Installation

Installation

```
// Install Dependencies
sudo apt-get update
sudo apt update && sudo apt upgrade -y && sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
sudo apt-get upgrade

// Install Go
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.24.2.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)

// Download binaries
cd $HOME
rm -rf Achilles
git clone https://github.com/daodiseomoney/Achilles.git
cd Achilles/achilles
git checkout v1.0.1
make install

mkdir -p $HOME/.achilles/cosmovisor/genesis/bin
wget -O $HOME/.achilles/cosmovisor/genesis/bin/achillesd https://green.codeblocklabs.com/testnet/odiseo/achillesd
chmod +x $HOME/.achilles/cosmovisor/genesis/bin/achillesd 

// Create application symlinks
ln -s $HOME/.achilles/cosmovisor/genesis $HOME/.achilles/cosmovisor/current -f
sudo ln -s $HOME/.achilles/cosmovisor/current/bin/achillesd /usr/local/bin/achilles -f
```

***

## Setting Up Services

```
# Download and install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0

# Create service
sudo tee /etc/systemd/system/achilles.service > /dev/null << EOF
[Unit]
Description=odiseo node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.achilles"
Environment="DAEMON_NAME=achillesd"
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
lumerad config chain-id ithaca-1
lumerad config keyring-backend test
lumerad config node tcp://localhost:10049

# Initialize the node
lumerad init YourNodesName --chain-id ithaca-1

# Download genesis and addrbook
curl -Ls https://green.codeblocklabs.com/testnet/odiseo/genesis.json > $HOME/.achilles/config/genesis.json
curl -Ls https://green.codeblocklabs.com/testnet/odiseo/addrbook.json > $HOME/.achilles/config/addrbook.json

# Add seeds
sed -i -e "s|^seeds *=.*|seeds = \"fed2bf1d2ec1714b1fe3889c2379ef72bff74646@135.181.238.225:19156\"|" $HOME/.achilles/config/config.toml

# Set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25uodis\"|" $HOME/.achilles/config/app.toml

# Set pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.lumera/config/app.toml

# Set custom ports (use any)
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:16958\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:16957\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:16960\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:16956\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":16966\"%" $HOME/.lumera/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:16917\"%; s%^address = \":8080\"%address = \":16980\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:16990\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:16991\"%; s%:8545%:16945%; s%:8546%:16946%; s%:6065%:16965%" $HOME/.lumera/config/app.toml

```

## Chain Snapshots

```
curl -L https://green.codeblocklabs.com/testnet/odiseo/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.achilles
```

```
// Start the nodes on services
sudo systemctl start achilles.service && sudo journalctl -u achilles.service -f --no-hostname -o cat
```



