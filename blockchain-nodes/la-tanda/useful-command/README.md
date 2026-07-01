# Useful Command

## Useful Command

### Wallet Commands

```bash
# Create a new wallet
latandad keys add wallet --home $HOME/.latanda

# Restore existing wallet
latandad keys add wallet --recover --home $HOME/.latanda

# Check sync status
latandad status 2>&1 | jq .sync_info --home $HOME/.latanda
```

### Create Validator File

```bash
tee $HOME/validator.json > /dev/null << EOF
{
	"pubkey": $(latandad comet show-validator --home $HOME/.latanda),
	"amount":  "1000000ultd",
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
latandad tx staking create-validator $HOME/validator.json --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Remove Node

```bash
sudo systemctl stop latandad
sudo systemctl disable latandad
sudo rm -rf /etc/systemd/system/latandad.service
sudo rm $(which latandad)
sudo rm -rf $HOME/.latanda
```
