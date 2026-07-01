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
wget https://github.com/INDIGOAZUL/la-tanda-chain -O latandad
chmod +x latandad
mv latandad ~/go/bin/
```

### Init Node

```bash
latandad init "Your_Nodes_Name" --chain-id latanda-testnet-1 --home $HOME/.latanda
```

### Download Genesis and Addrbook

```bash
wget -O $HOME/.latanda/config/genesis.json https://backup.codeblocklabs.com/latanda-testnet-1/genesis.json
wget -O $HOME/.latanda/config/addrbook.json https://backup.codeblocklabs.com/latanda-testnet-1/addrbook.json
```

### Set Custom Ports

```bash
# Set ports in app.toml
sed -i -e "s%:1317%:12517%; s%:8080%:12580%; s%:9090%:12590%; s%:9091%:12591%; s%:8545%:12545%; s%:8546%:12546%; s%:6065%:12565%" $HOME/.latanda/config/app.toml

# Set ports in config.toml
sed -i -e "s%:26658%:12558%; s%:26657%:12557%; s%:6060%:12560%; s%:26656%:12556%; s%:26660%:12560%" $HOME/.latanda/config/config.toml

# Set node in client.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:12557\"|" $HOME/.latanda/config/client.toml
```

### Config Pruning

```bash
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.latanda/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.latanda/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.latanda/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.latanda/config/app.toml
```

### Set Minimum Gas Price

```bash
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25ultd\"|" $HOME/.latanda/config/app.toml
```

### Set Persistent Peers

```bash
peers="YOUR_PEERS_HERE"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.latanda/config/config.toml
```

### Disable Indexing

```bash
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.latanda/config/config.toml
```

### Enable and Start Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable latandad
sudo systemctl restart latandad && sudo journalctl -u latandad -f
```
