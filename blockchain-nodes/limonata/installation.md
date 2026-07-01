# Installation

## Installation

### Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

### Install Go

```bash
cd $HOME
sudo rm -rf /usr/local/go
VER="1.24.5"
curl -Ls https://go.dev/dl/go$VER.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
mkdir -p ~/go/bin
go version
```

### Download Binary

```bash
cd $HOME
mkdir -p ~/go/bin
wget https://github.com/Limonata-Blockchain/limonata -O limonatad
chmod +x limonatad
mv limonatad ~/go/bin/
```

### Init Node

```bash
limonatad init "Your_Nodes_Name" --chain-id limonata_10777-1 --home $HOME/.limonata
```

### Download Genesis and Addrbook

```bash
wget -O $HOME/.limonata/config/genesis.json https://backup.codeblocklabs.com/limonata_10777-1/genesis.json
wget -O $HOME/.limonata/config/addrbook.json https://backup.codeblocklabs.com/limonata_10777-1/addrbook.json
```

### Set Custom Ports

```bash
# Set ports in app.toml
sed -i -e "s%:1317%:12417%; s%:8080%:12480%; s%:9090%:12490%; s%:9091%:12491%; s%:8545%:12445%; s%:8546%:12446%; s%:6065%:12465%" $HOME/.limonata/config/app.toml

# Set ports in config.toml
sed -i -e "s%:26658%:12458%; s%:26657%:12457%; s%:6060%:12460%; s%:26656%:12456%; s%:26660%:12460%" $HOME/.limonata/config/config.toml

# Set node in client.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:12457\"|" $HOME/.limonata/config/client.toml
```

### Config Pruning

```bash
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.limonata/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.limonata/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.limonata/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.limonata/config/app.toml
```

### Set Minimum Gas Price

```bash
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25alimo\"|" $HOME/.limonata/config/app.toml
```

### Set Persistent Peers

```bash
peers="YOUR_PEERS_HERE"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.limonata/config/config.toml
```

### Disable Indexing

```bash
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.limonata/config/config.toml
```

### Enable and Start Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable limonatad
sudo systemctl restart limonatad && sudo journalctl -u limonatad -f
```
