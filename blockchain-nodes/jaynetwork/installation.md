# Installation

```bash
# Install Dependencies
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

```bash
# install go, if needed
cd $HOME
sudo rm -rf /usr/local/go
VER="1.24.5"
curl -Ls https://go.dev/dl/go$VER.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
mkdir -p ~/go/bin
go version
```

```bash
# Download Binary
cd $HOME
mkdir -p ~/go/bin
wget https://github.com/bbtccore/thejaynetwork/releases/download/v3-static/jaynd-linux-amd64 -O jaynd
chmod +x jaynd
mv jaynd ~/go/bin/
```

```bash
# config and init app
jaynd init "Your_Nodes_Name" --chain-id thejaynetwork
```

```bash
# download genesis and addrbook
wget -O $HOME/.jayn/config/genesis.json https://backup.codeblocklabs.com/thejaynetwork/genesis.json
wget -O $HOME/.jayn/config/addrbook.json https://backup.codeblocklabs.com/thejaynetwork/addrbook.json
```

```bash
# Set custom ports in app.toml
sed -i -e "s%:1317%:11117%; s%:8080%:11180%; s%:9090%:11190%; s%:9091%:11191%; s%:8545%:11145%; s%:8546%:11146%; s%:6065%:11165%" $HOME/.jayn/config/app.toml
sed -i -e "s%:26658%:11158%; s%:26657%:11157%; s%:6060%:11160%; s%:26656%:11156%; s%:26660%:11160%" $HOME/.jayn/config/config.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:11157\"|" $HOME/.jayn/config/client.toml
```

```bash
# config pruning
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.jayn/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.jayn/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.jayn/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.jayn/config/app.toml
```

```bash
# set minimum gas price
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.025ujay\"|" $HOME/.jayn/config/app.toml
```

```bash
# set persistent peers
peers="218942ae4b1d1d8ab6517b06e0828198a0867bb1@65.21.234.111:18256,ba900c17cfcc187e374323ff31016b178d088d55@89.58.24.232:26656,9951f5546f067ed877dc425fadc98e234e0480c1@152.53.195.74:26656"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.jayn/config/config.toml
```

```bash
# Disable indexing
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.jayn/config/config.toml
```

{% tabs %}
{% tab title="Cosmovisor" %}
<pre><code><strong>mkdir -p $HOME/.jayn/cosmovisor/genesis/bin
</strong>mv $HOME/go/bin/jaynd $HOME/.jayn/cosmovisor/genesis/bin/

# Create symlinks
sudo ln -s $HOME/.jayn/cosmovisor/genesis $HOME/.jayn/cosmovisor/current -f
sudo ln -s $HOME/.jayn/cosmovisor/current/bin/jaynd /usr/local/bin/jaynd -f

# Install cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.7.0

# Create systemd service
sudo tee /etc/systemd/system/jaynd.service > /dev/null &#x3C;&#x3C; EOF
[Unit]
Description=jayn node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.jayn"
Environment="DAEMON_NAME=jaynd"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
</code></pre>
{% endtab %}

{% tab title="Non Cosmovisor" %}
```
sudo tee /etc/systemd/system/republicd.service > /dev/null <<EOF
[Unit]
Description=Republic node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.republic
ExecStart=$(which republicd) start --home $HOME/.republic
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
{% endtab %}
{% endtabs %}

```bash
# # enable and start service
sudo systemctl daemon-reload
sudo systemctl enable jaynd
sudo systemctl restart jaynd && sudo journalctl -u jaynd -f
```

