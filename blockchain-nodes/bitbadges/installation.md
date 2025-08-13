---
description: Bitbadges Mainnet
---

# Installation

### Step 1: Update & Install Dependencies

```bash
bashsudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

### Step 2: Install Golang

```bash
bashcd $HOME
GO_VERSION="1.24.5"
wget "https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go${GO_VERSION}.linux-amd64.tar.gz"
rm "go${GO_VERSION}.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

### Step 3: Setup Environment Variables

```bash
bashecho "export BITBADGES_WALLET="wallet"" >> $HOME/.bash_profile
echo "export BITBADGES_MONIKER="test"" >> $HOME/.bash_profile
echo "export BITBADGES_CHAIN_ID="bitbadges-1"" >> $HOME/.bash_profile
echo "export BITBADGES_PORT="13"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Step 4: Install Bitbadges Binary

```bash
bashcd $HOME
mkdir -p $HOME/.bitbadgeschain
wget https://github.com/BitBadges/bitbadgeschain/releases/download/v12/bitbadgeschain-linux-amd64 -O $HOME/go/bin/bitbadgeschaind
chmod +x $HOME/go/bin/bitbadgeschaind
```

```bash
bashcd /usr/lib
sudo curl -L -o libwasmvm.x86_64.so https://github.com/CosmWasm/wasmvm/releases/download/v1.2.3/libwasmvm.x86_64.so
bash
sudo ldconfig
```

### Step 5: Initialize the Node

```bash
bashbitbadgeschaind init $BITBADGES_MONIKER --chain-id $BITBADGES_CHAIN_ID
```

### Step 6: Download Genesis & Addrbook File

```bash
bashcurl -Ls https://data.winsnip.xyz/list/mainnet/bitbadges/genesis.json > $HOME/.bitbadgeschain/config/genesis.json
curl -Ls https://data.winsnip.xyz/list/mainnet/bitbadges/addrbook.json > $HOME/.bitbadgeschain/config/addrbook.json
```

### Step 7: Configure Node

```bash
bash# Change default ports
sed -i.bak -e "s%:1317%:${BITBADGES_PORT}317%g;
s%:8080%:${BITBADGES_PORT}080%g;
s%:9090%:${BITBADGES_PORT}090%g;
s%:9091%:${BITBADGES_PORT}091%g;
s%:8545%:${BITBADGES_PORT}545%g;
s%:8546%:${BITBADGES_PORT}546%g;
s%:6065%:${BITBADGES_PORT}065%g" $HOME/.bitbadgeschain/config/app.toml

sed -i.bak -e "s%:26658%:${BITBADGES_PORT}658%g;
s%:26657%:${BITBADGES_PORT}657%g;
s%:6060%:${BITBADGES_PORT}060%g;
s%:26656%:${BITBADGES_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${BITBADGES_PORT}656\"%;
s%:26660%:${BITBADGES_PORT}660%g" $HOME/.bitbadgeschain/config/config.toml
```

### Step 8: Set Pruning & Prometheus

```bash
bashsed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.bitbadgeschain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.bitbadgeschain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"19\"/" $HOME/.bitbadgeschain/config/app.toml

sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.bitbadgeschain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.bitbadgeschain/config/config.toml
```

### Step 9: Create Systemd Service

```bash
bashsudo tee /etc/systemd/system/bitbadgeschaind.service > /dev/null <<EOF
[Unit]
Description=Bitbadges node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.bitbadgeschain
ExecStart=$(which bitbadgeschaind) start --home $HOME/.bitbadgeschain
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

### Step 10: Start Service

```bash
bashbitbadgeschaind tendermint unsafe-reset-all --home $HOME/.bitbadgeschain
sudo systemctl daemon-reload
sudo systemctl enable bitbadgeschaind
sudo systemctl restart bitbadgeschaind
```

### Step 11: Monitor Node Status

```bash
bashsudo journalctl -u bitbadgeschaind -fo cat
```
