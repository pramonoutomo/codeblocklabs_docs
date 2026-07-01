# Useful Command

## Useful Command

### Wallet Commands

```bash
# Create a new wallet
limonatad keys add wallet --home $HOME/.limonata

# Restore existing wallet
limonatad keys add wallet --recover --home $HOME/.limonata

# Check sync status
limonatad status 2>&1 | jq .sync_info --home $HOME/.limonata
```

### Create Validator File

```bash
tee $HOME/validator.json > /dev/null << EOF
{
	"pubkey": $(limonatad comet show-validator --home $HOME/.limonata),
	"amount":  "1000000alimo",
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
limonatad tx staking create-validator $HOME/validator.json --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Remove Node

```bash
sudo systemctl stop limonatad
sudo systemctl disable limonatad
sudo rm -rf /etc/systemd/system/limonatad.service
sudo rm $(which limonatad)
sudo rm -rf $HOME/.limonata
```
