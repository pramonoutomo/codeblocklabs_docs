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
wget https://github.com/qieadmin/qiev3-mainnet/releases/download/v3/qiemainnetv3.zip -O qied
chmod +x qied
mv qied ~/go/bin/
```

### Init Node

```bash
qied init "Your_Nodes_Name" --chain-id qie_1990-1 --home $HOME/.qie
```

### Download Genesis and Addrbook

```bash
wget -O $HOME/.qie/config/genesis.json https://backup.codeblocklabs.com/qie_1990-1/genesis.json
wget -O $HOME/.qie/config/addrbook.json https://backup.codeblocklabs.com/qie_1990-1/addrbook.json
```

### Set Custom Ports

```bash
# Set ports in app.toml
sed -i -e "s%:1317%:12317%; s%:8080%:12380%; s%:9090%:12390%; s%:9091%:12391%; s%:8545%:12345%; s%:8546%:12346%; s%:6065%:12365%" $HOME/.qie/config/app.toml

# Set ports in config.toml
sed -i -e "s%:26658%:12358%; s%:26657%:12357%; s%:6060%:12360%; s%:26656%:12356%; s%:26660%:12360%" $HOME/.qie/config/config.toml

# Set node in client.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:12357\"|" $HOME/.qie/config/client.toml
```

### Config Pruning

```bash
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.qie/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.qie/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.qie/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.qie/config/app.toml
```

### Set Minimum Gas Price

```bash
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.25aqie\"|" $HOME/.qie/config/app.toml
```

### Set Persistent Peers

```bash
peers="YOUR_PEERS_HERE"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.qie/config/config.toml
```

### Disable Indexing

```bash
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.qie/config/config.toml
```

### Enable and Start Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable qied
sudo systemctl restart qied && sudo journalctl -u qied -f
```
