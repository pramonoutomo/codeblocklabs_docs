---
description: Elys Mainnet Installation
---

# Installation

#### Setup Server <a href="#setup" id="setup"></a>

```bash
# Update Packages and Install Prerequisites
sudo apt update && apt upgrade -y
sudo apt install curl git jq lz4 build-essential unzip fail2ban ufw -y
```

```bash
# Set Firewall
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw allow 9100
```

```bash
# Enable Firewall
sudo ufw enable
```

#### Install Elys Daemon

```
# Install elys through repository
cd $HOME
rm -rf elys
git clone https://github.com/elys-network/elys.git elys
cd elys
git checkout v2.3.0
make install
```

#### Setting Up Your Node

```
# config
elysd config chain-id elys-1
elysd config keyring-backend file

# init, make sure to change your NodeName
elysd init NodeName --chain-id elys-1

# Add Genesis File and Addrbook
curl -Ls https://snapshots.indonode.net/elys/genesis.json > $HOME/.elys/config/genesis.json
curl -Ls https://snapshots.indonode.net/elys/addrbook.json > $HOME/.elys/config/addrbook.json

# Configure Seeds and Peers
PEERS="8186ee0b11f74bb7e28f99b6c27dff498904bb68@185.119.118.114:6000,6a748ef8164e501223c830c0ad79b331a05ef16a@141.94.199.25:36442,ba252ed5b0ee59a80f1c147dc43c27dc17e9a54e@51.83.140.142:30856,e93fbb087acb7c0f8ca850a796310bb745b510b6@23.227.223.249:26656,303c2632596a00cdec34205ab612f33def631b49@195.154.100.227:56656,d71d3bce45274bf8354298042674a08c778f6d27@202.61.243.56:22056,d95bdf717eb751667586b5e31083770630742038@65.109.58.158:22156,c8703ffc49271d7951aebce09e8419f1066b89af@65.108.30.59:32656,1d079e8b757b21b390f3eca0880ca03f7f90d8f0@95.217.143.167:20656,12f932304c5d8c60280cec4bf0dcf4e4b99e454f@169.0.32.67:26656,380048bb45143b2b87c540c772886f5a08bae344@86.90.185.145:26156,36a24ccff963cd7339ec3178e4a5ad33c285d553@149.56.240.152:26656,6aaf6c59af92adde0ef65cdb43ce281609ad348a@37.27.53.176:22056,77b3ee6202e3508f705229bbb068f8199275bd29@65.108.71.137:22056,637077d431f618181597706810a65c826524fd74@65.109.111.111:22056,45afc781e9afa4dfe6db2f2afedd1e476eac2fb3@23.227.222.185:32556,6185b8cbf01c697d116c84378eecc8f433024065@144.76.115.182:26656,d9bfa29e0cf9c4ce0cc9c26d98e5d97228f93b0b@37.27.61.38:15356,b4fca2e4f5c017a817d36b2a8e214b74dc436e7f@65.109.23.55:31126,df2b8a0137aba24ec652ccb8e44a2823a67c56c5@144.76.217.47:26656,b6f3ed7ecefdaad90be7b963ba75eb583c367f0c@135.181.238.225:10056,43699fd956ac02a90aed34ed49b6da125a2b55ab@144.76.217.227:26656,2b8417ec0e27025a10b494a71b92349e5e38487c@65.109.115.172:22056,c5f791d8385a5d7e946f2fc659489fd4c8e51fbd@73.40.158.140:33656,60209d3b252bd07229530d4d64591a181b7f0483@65.109.25.246:31056,89000b116f3f9485ab091b1ab5513b70fc2f9c08@91.134.9.162:22056,9b9dee928a174bcd0272be9127f5f455d418d6b2@102.182.201.193:26656,e1b058e5cfa2b836ddaa496b10911da62dcf182e@164.152.161.168:26656,e726816f42831689eab9378d5d577f1d06d25716@169.155.171.162:26656,b42c60adafb47b19f0b36c58985364a897db6b85@65.109.26.242:22056,db9a5cd7e5adcfad626a416db390567ae0d840b1@149.102.155.91:22056,1af6af8fa99d70c3f3a69ae48d30c8d8a52dbf7b@64.120.114.5:22056,effbd226550543e98baa7c8b3aa0e5b5d6a9a3c6@164.152.161.131:32556,6a7b53154be0d4a161bdc732b0fecdf6c4da810c@65.109.30.116:6000,1da47479beaf73dd16195eb35e56d643ec4c8885@213.239.198.181:46656,36d9c4dcdee38411d680f897481322bcb7e10aa8@65.108.75.179:41656,3f30f68cb08e4dae5dd76c5ce77e6e1a15084346@167.235.21.165:36656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.elys/config/config.toml

```

#### Setting Pruning, Minimum Gas Prices and Indexer (Optional)

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
$HOME/.elys/config/app.toml
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.elys/config/config.toml
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.0003uelys\"|" $HOME/.elys/config/app.toml
```

Setup Cosmovisor

```
// install cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest

// make genesis directory
mkdir -p ~/.elys/cosmovisor/genesis/bin
// make upgrades directory
mkdir -p ~/.elys/cosmovisor/upgrades

// get elys daemon location
which elysd
// you should now getting the elysd location, for example like: ~/go/bin/elysd
// copy to genesis directory
cp YourBinaryLocation ~/.elys/cosmovisor/genesis/bin/ 

```

#### Running as services with cosmovisor

<pre><code>// Get your cosmovisor binary location
which cosmovisor
// you should now getting the cosmovisor location, for example like: ~/go/bin/cosmovisor
// save this cosmovisor binary location

// create services
<strong>sudo nano /etc/systemd/system/elys.service
</strong></code></pre>

{% code title="elys.service" %}
```
    [Unit] 
    Description=Elys Network node 
    After=network.target

    [Service] 
    Type=simple 
    Restart=on-failure 
    RestartSec=5 
    User=YOUR_USERNAME 
    ExecStart=YOUR_COSMOVISOR_BINARY_LOCATION run start
    LimitNOFILE=65535
    Environment="DAEMON_NAME=elysd"
    Environment="DAEMON_HOME=YOUR_DATA_FOLDER/.elys"
    Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
    Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
    Environment="UNSAFE_SKIP_BACKUP=true"

    [Install] 
    WantedBy=multi-user.target
```
{% endcode %}

\*\*save the file, make sure to change YOUR\_USERNAME, YOUR\_COSMOVISOR\_BINARY\_LOCATION, YOUR\_DATA\_FOLDER.

```
// Reload Daemon
sudo systemctl daemon-reload
// Enable Services
systemctl enable elys.service
// To check logs, use: journalctl -fu elys

```

***

Using Snapshots

```
sudo apt update
sudo apt-get install snapd lz4 -y
```

The latest snapshot : `elys_snapshot_(todaydate).tar.lz4`

```
// Download
curl -L elys_snapshot_(todaydate).tar.lz4 | tar -Ilz4 -xf - -C $HOME/.elys

```

{% hint style="info" %}
**Useful Tips**\
&#xNAN;_&#x43;heck Version:_ blockxd version --long

_CHECK STATUS BINARY:_ systemctl status blockxd \
&#xNAN;_&#x43;HECK RUNNING LOGS:_ journalctl -fu blockxd -o cat \
&#xNAN;_&#x43;HECK LOCAL STATUS:_ curl -s localhost:26657/status | jq .result.sync\_info
{% endhint %}
