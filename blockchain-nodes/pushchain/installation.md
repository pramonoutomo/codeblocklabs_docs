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
wget https://github.com/pushchain/push-chain-node -O pchaind
chmod +x pchaind
mv pchaind ~/go/bin/
```

### Init Node

```bash
pchaind init "Your_Nodes_Name" --chain-id push_42101-1 --home $HOME/.pushchain
```

### Download Genesis and Addrbook

```bash
wget -O $HOME/.pushchain/config/genesis.json https://backup.codeblocklabs.com/push_42101-1/genesis.json
wget -O $HOME/.pushchain/config/addrbook.json https://backup.codeblocklabs.com/push_42101-1/addrbook.json
```

### Set Custom Ports

```bash
# Set ports in app.toml
sed -i -e "s%:1317%:11417%; s%:8080%:11480%; s%:9090%:11490%; s%:9091%:11491%; s%:8545%:11445%; s%:8546%:11446%; s%:6065%:11465%" $HOME/.pushchain/config/app.toml

# Set ports in config.toml
sed -i -e "s%:26658%:11458%; s%:26657%:11457%; s%:6060%:11460%; s%:26656%:11456%; s%:26660%:11460%" $HOME/.pushchain/config/config.toml

# Set node in client.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:11457\"|" $HOME/.pushchain/config/client.toml
```

### Config Pruning

```bash
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.pushchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.pushchain/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.pushchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.pushchain/config/app.toml
```

### Set Minimum Gas Price

```bash
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25upc\"|" $HOME/.pushchain/config/app.toml
```

### Set Persistent Peers

```bash
peers="YOUR_PEERS_HERE"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.pushchain/config/config.toml
```

### Disable Indexing

```bash
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.pushchain/config/config.toml
```

### Enable and Start Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable pchaind
sudo systemctl restart pchaind && sudo journalctl -u pchaind -f
```
