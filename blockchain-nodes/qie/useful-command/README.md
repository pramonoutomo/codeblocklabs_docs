# Useful Command

## Useful Command

### Wallet Commands

```bash
# Create a new wallet
qied keys add wallet --home $HOME/.qie

# Restore existing wallet
qied keys add wallet --recover --home $HOME/.qie

# Check sync status
qied status 2>&1 | jq .sync_info --home $HOME/.qie
```

### Create Validator File

```bash
tee $HOME/validator.json > /dev/null << EOF
{
	"pubkey": $(qied comet show-validator --home $HOME/.qie),
	"amount":  "1000000aqie",
	"moniker": "Your_Nodes_Name",
	"identity": "Your_Keybase",
	"website": "Your_Website",
	"details": "Your_Detail",
	"commission-rate": "0.05",
	"commission-max-rate": "0.2",
	"commission-max-change-rate": "0.05",
	"min-self-delegation": "1"
}
EOF
```

### Deploy Validator

```bash
qied tx staking create-validator $HOME/validator.json --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Remove Node

```bash
sudo systemctl stop qied
sudo systemctl disable qied
sudo rm -rf /etc/systemd/system/qied.service
sudo rm $(which qied)
sudo rm -rf $HOME/.qie
```
