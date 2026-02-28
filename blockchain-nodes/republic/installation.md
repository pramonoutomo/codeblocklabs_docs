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
VER="1.25.5"
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
wget https://github.com/RepublicAI/networks/raw/refs/heads/main/testnet/releases/v0.3.0/republicd-linux-amd64 -O republicd
chmod +x republicd
mv republicd ~/go/bin/
```

```bash
# config and init app
republicd init "Your_Nodes_Name" --chain-id raitestnet_77701-1
```

```bash
# download genesis and addrbook
wget -O $HOME/.republic/config/genesis.json http://45.76.1.67/republic/genesis.json
wget -O $HOME/.republic/config/addrbook.json http://45.76.1.67/republic/addrbook.json
```

```bash
# Set custom ports in app.toml
sed -i -e "s%:1317%:30317%; s%:8080%:30080%; s%:9090%:30090%; s%:9091%:30091%; s%:8545%:30545%; s%:8546%:30546%; s%:6065%:30065%" $HOME/.republic/config/app.toml
sed -i -e "s%:26658%:30658%; s%:26657%:30657%; s%:6060%:30060%; s%:26656%:30656%; s%:26660%:30660%" $HOME/.republic/config/config.toml
sed -i -e "s|^node *=.*|node = \"tcp://localhost:30657\"|" $HOME/.republic/config/client.toml
```

```bash
# config pruning
pruning="custom"
pruning_keep_every="0"
pruning_keep_recent="100"
pruning_interval="19"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.republic/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.republic/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.republic/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.republic/config/app.toml
```

```bash
# set minimum gas price
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "2500000000arai"|g' $HOME/.republic/config/app.toml
```

```bash
# Disable indexing
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.republic/config/config.toml
```

{% tabs %}
{% tab title="Cosmovisor" %}
<pre><code><strong>mkdir -p $HOME/.republic/cosmovisor/genesis/bin
</strong>mv $HOME/go/bin/republicd $HOME/.republic/cosmovisor/genesis/bin/

# Create symlinks
sudo ln -s $HOME/.republic/cosmovisor/genesis $HOME/.republic/cosmovisor/current -f
sudo ln -s $HOME/.republic/cosmovisor/current/bin/republicd /usr/local/bin/republicd -f

# Install cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.7.0

# Create systemd service
sudo tee /etc/systemd/system/republicd.service > /dev/null &#x3C;&#x3C; EOF
[Unit]
Description=republic node service
After=network-online.target

[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.republic"
Environment="DAEMON_NAME=republicd"
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
sudo systemctl enable republicd
sudo systemctl restart republicd && sudo journalctl -u republicd -f
```
