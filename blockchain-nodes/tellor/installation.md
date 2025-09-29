# Installation

1\. System Preparation

```
# System update
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install -y build-essential jq sed curl wget git make gcc g++ unzip snapd lz4
```

2\. Go Installation

```
cd $HOME
wget https://go.dev/dl/go1.23.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.23.3.linux-amd64.tar.gz
rm go1.23.3.linux-amd64.tar.gz

# PATH configuration
cat << 'EOF' >> ~/.bashrc
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
export GOPATH=$HOME/go
EOF

source ~/.bashrc
go version
```

3\. Binary Download and Installation

```
# Create binary directory
mkdir -p ~/layer/binaries/v5.1.1 && cd ~/layer/binaries/v5.1.1

# Download binary
wget https://github.com/tellor-io/layer/releases/download/v5.1.1/layer_Linux_x86_64.tar.gz
tar -xvzf layer_Linux_x86_64.tar.gz

# Copy to system-wide location
sudo cp layerd /usr/local/bin/
sudo chmod +x /usr/local/bin/layerd

# Version check
layerd version
```

4\. Node Initialization

```
# Initialize node (replace YOUR_NODE_NAME with your own name)
layerd init "YOUR_NODE_NAME" --chain-id tellor-1

# Download genesis file
curl -Ls https://ss.tellor.nodestake.org/genesis.json > $HOME/.layer/config/genesis.json
```

5\. Port Configuration

```
# Change CUSTOM_PORT value for custom port (default: 45)
CUSTOM_PORT=45

# Port settings
sed -i -e "s|tcp://127.0.0.1:26657|tcp://0.0.0.0:${CUSTOM_PORT}657|g" $HOME/.layer/config/config.toml
sed -i -e "s|:26656|:${CUSTOM_PORT}656|g" $HOME/.layer/config/config.toml
sed -i -e "s|:26658|:${CUSTOM_PORT}658|g" $HOME/.layer/config/config.toml
sed -i -e "s|proxy_app = \"tcp://127.0.0.1:26658\"|proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"|g" $HOME/.layer/config/config.toml
sed -i -e "s|tcp://localhost:26657|tcp://localhost:${CUSTOM_PORT}657|g" $HOME/.layer/config/client.toml
```

6\. Configuration Filesapp.toml Settings

```
# Gas prices
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0loya\"/" $HOME/.layer/config/app.toml

# Enable API
sed -i -e "s/^enable *=.*/enable = true/" $HOME/.layer/config/app.toml
sed -i -e "s/^swagger *=.*/swagger = true/" $HOME/.layer/config/app.toml

# Pruning settings
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.layer/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.layer/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.layer/config/app.toml
```

config.toml Settings

```
# Add peer list
PEERS="3037a8c239cdcdcf7fbc0ed050a11ecdc0397374@91.99.194.56:26656,17355981bc61dc3c4169158e3d73f22099a5f9c0@152.53.254.219:41767,5ef1ed1fec8700bf9ee16625db2718997ceb499d@157.180.52.245:41656,23a9da592ee6688eac45c82a256ef302a661469b@195.3.223.78:51656,95e55a6cfb850db8c23e969ddd461eac28b98702@3.91.103.4:26656,7fd4d34f3b19c41218027d3b91c90d073ab2ba66@54.221.149.61:26656,2737f23b2223ab1673ce682afdf50d34633f5f7c@69.250.123.126:26656,9358c72aa8be31ce151ef591e6ecf08d25812993@18.143.181.83:26656,2904aa32501548e127d3198c8f5181fb4d67bbe6@18.116.23.104:26656,2b8af463a1f0e84aec6e4dbf3126edf3225df85e@13.52.231.70:26656,f2644778a8a2ca3b55ec65f1b7799d32d4a7098e@54.149.160.93:26656,5a9db46eceb055c9238833aa54e15a2a32a09c9a@54.67.36.145:26656"

sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.layer/config/config.toml

# Other settings
sed -i -e "s/^cors_allowed_origins *=.*/cors_allowed_origins = [\"*\"]/" $HOME/.layer/config/config.toml
sed -i -e "s/^timeout_commit *=.*/timeout_commit = \"1s\"/" $HOME/.layer/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"kv\"/" $HOME/.layer/config/config.toml
```

client.toml Settings

```
sed -i -e "s/^chain-id *=.*/chain-id = \"tellor-1\"/" $HOME/.layer/config/client.toml
sed -i -e "s/^keyring-backend *=.*/keyring-backend = \"test\"/" $HOME/.layer/config/client.toml
```

7\. Wallet Creation

```
# Create new wallet (save your seed phrase!)
layerd keys add wallet --keyring-backend test

# Display wallet address
layerd keys show wallet -a --keyring-backend test
```

8\. Ethereum RPC Settings

```
# Get API key from Alchemy or Infura and replace
ETH_RPC_URL="wss://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY"

# Add environment variables
cat << EOF >> ~/.bashrc
export ETH_RPC_URL="$ETH_RPC_URL"
export ETH_RPC_URL_PRIMARY="$ETH_RPC_URL"
export ETH_RPC_URL_FALLBACK="${ETH_RPC_URL/wss/https}"
export TOKEN_BRIDGE_CONTRACT="0x5589e306b1920F009979a50B88caE32aecD471E4"
EOF

source ~/.bashrc
```

9\. Cosmovisor Installation

```
# Install Cosmovisor
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest

# Environment variables
cat << 'EOF' >> ~/.bashrc
export DAEMON_NAME=layerd
export DAEMON_HOME=$HOME/.layer
export DAEMON_RESTART_AFTER_UPGRADE=true
export DAEMON_ALLOW_DOWNLOAD_BINARIES=false
export DAEMON_POLL_INTERVAL=300ms
export UNSAFE_SKIP_BACKUP=true
export DAEMON_PREUPGRADE_MAX_RETRIES=0
EOF

source ~/.bashrc

# Create directory structure
mkdir -p $HOME/.layer/cosmovisor/genesis/bin
cp /usr/local/bin/layerd $HOME/.layer/cosmovisor/genesis/bin/
chmod +x $HOME/.layer/cosmovisor/genesis/bin/layerd

# Test
cosmovisor version
```

10\. Snapshot Download (Fast Sync)

```
# Clean data directory
rm -rf $HOME/.layer/data
layerd tendermint unsafe-reset-all --home $HOME/.layer/ --keep-addr-book

# Download snapshot (30+ GB, be patient)
echo "Downloading snapshot..."
SNAP_NAME=$(curl -s https://ss.tellor.nodestake.org/ | egrep -o ">20.*\.tar.lz4" | tr -d ">")
curl -o - -L https://ss.tellor.nodestake.org/${SNAP_NAME} | lz4 -c -d - | tar -x -C $HOME/.layer

echo "Snapshot successfully loaded!"
```

11\. Create Systemd Service

```
sudo tee /etc/systemd/system/tellor.service > /dev/null <<EOF
[Unit]
Description=Tellor Layer Node
After=network-online.target

[Service]
User=$USER
WorkingDirectory=$HOME
ExecStart=$HOME/go/bin/cosmovisor run start --home $HOME/.layer --keyring-backend test --key-name wallet --api.enable --api.swagger
Restart=always
RestartSec=3
LimitNOFILE=65535
Environment="DAEMON_NAME=layerd"
Environment="DAEMON_HOME=$HOME/.layer"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="ETH_RPC_URL=$ETH_RPC_URL"
Environment="ETH_RPC_URL_PRIMARY=$ETH_RPC_URL"
Environment="ETH_RPC_URL_FALLBACK=${ETH_RPC_URL/wss/https}"
Environment="TOKEN_BRIDGE_CONTRACT=0x5589e306b1920F009979a50B88caE32aecD471E4"

[Install]
WantedBy=multi-user.target
EOF

# Start service
sudo systemctl daemon-reload
sudo systemctl enable tellor
sudo systemctl start tellor
```

12\. Check Node Status

```
# Watch logs
sudo journalctl -u tellor -f

# Wait 30 seconds, then check status
sleep 30
curl -s localhost:45657/status | jq '.result.sync_info | {height: .latest_block_height, catching_up: .catching_up}'

# Check peer count
curl -s localhost:45657/net_info | jq -r '.result.n_peers'

# Check current block height
curl -s https://mainnet.tellorlayer.com/rpc/status | jq -r '.result.sync_info.latest_block_height'
```

13\. Validator CreationPrerequisites

* Node must be fully synced (catching\_up: false)
* Minimum 1 TRB token required
* Bridge your TRB tokens from https://bridge.tellor.io

Validator Creation Steps1. Balance Check

```
# Show wallet address
layerd keys show wallet -a --keyring-backend test

# Check balance (minimum 1 TRB = 1000000 loya required)
layerd query bank balances $(layerd keys show wallet -a --keyring-backend test)
```

2\. Create Validator JSON File

```
# Get your validator public key
VALIDATOR_PUBKEY=$(layerd tendermint show-validator)

# Create validator JSON file (edit information)
cat > tellor-validator.json << EOF
{
  "pubkey": $VALIDATOR_PUBKEY,
  "amount": "1000000loya",
  "moniker": "YOUR_VALIDATOR_NAME",
  "identity": "YOUR_KEYBASE_ID",
  "website": "YOUR_WEBSITE",
  "security": "YOUR_EMAIL",
  "details": "YOUR_VALIDATOR_DESCRIPTION",
  "commission-rate": "0.1",
  "commission-max-rate": "0.2",
  "commission-max-change-rate": "0.01",
  "min-self-delegation": "1"
}
EOF
```

3\. Create Validator

```
# Create validator with JSON file (fee is mandatory!)
layerd tx staking create-validator tellor-validator.json \
  --from=wallet \
  --chain-id=tellor-1 \
  --keyring-backend=test \
  --fees="10000loya" \
  -y
```

4\. Check Validator Status

```
# Get your validator address
VALOPER=$(layerd keys show wallet --bech val -a --keyring-backend test)

# Check validator info
layerd query staking validator $VALOPER

# Check on explorer
echo "Explorer: https://tellorexplorer.com/validators/$VALOPER"
```

Useful CommandsService Management

```
sudo systemctl stop tellor      # Stop
sudo systemctl start tellor     # Start
sudo systemctl restart tellor   # Restart
sudo systemctl status tellor    # Check status
sudo journalctl -u tellor -f    # Watch logs
```

Wallet Operations

```
# Wallet list
layerd keys list --keyring-backend test

# Wallet address
layerd keys show wallet -a --keyring-backend test

# Balance check
layerd query bank balances $(layerd keys show wallet -a --keyring-backend test)
```

Node Information

```
# Sync status
layerd status 2>&1 | jq '.SyncInfo.catching_up'

# Node ID
layerd status 2>&1 | jq '.NodeInfo.id'

# Validator info
layerd query staking validator $(layerd keys show wallet --bech val -a --keyring-backend test)
```

Validator Operations

```
# Unjail
layerd tx slashing unjail \
  --from wallet \
  --chain-id tellor-1 \
  --keyring-backend test \
  --fees="10000loya" \
  -y

# Delegate (stake tokens)
layerd tx staking delegate $(layerd keys show wallet --bech val -a --keyring-backend test) 1000000loya \
  --from wallet \
  --chain-id tellor-1 \
  --keyring-backend test \
  --fees="10000loya" \
  -y
```

TroubleshootingNode not starting

```
# Check logs
sudo journalctl -u tellor -n 100

# Check port conflicts
sudo lsof -i :45657
```

Slow synchronization

```
# Check peer count (should be 10+)
curl -s localhost:45657/net_info | jq -r '.result.n_peers'

# Download new snapshot
sudo systemctl stop tellor
rm -rf $HOME/.layer/data
# Repeat snapshot download step
```

Validator not visible

```
# Check jail status
layerd query staking validator $(layerd keys show wallet --bech val -a --keyring-backend test) | jq '.jailed'

# Check if TX was successful
layerd query tx TX_HASH
```

Important Notes

* 🔴 **Always save your seed phrase in a secure location!**
* 🟡 **Ethereum RPC requires Alchemy or Infura API key**
* 🟢 **Edit CUSTOM\_PORT value to change ports**
* 🔵 **Minimum 1 TRB (1000000 loya) required for validator**
* ⚪ **Gas fee is mandatory (--fees="10000loya")**
* 🟣 **Snapshot download is 30+ GB, be patient**

\
