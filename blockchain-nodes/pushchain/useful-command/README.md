# Useful Command

## Useful Command

### Wallet Commands

```bash
# Create a new wallet
pchaind keys add wallet --home $HOME/.pushchain

# Restore existing wallet
pchaind keys add wallet --recover --home $HOME/.pushchain

# Check sync status
pchaind status 2>&1 | jq .sync_info --home $HOME/.pushchain
```

### Create Validator File

```bash
tee $HOME/validator.json > /dev/null << EOF
{
	"pubkey": $(pchaind comet show-validator --home $HOME/.pushchain),
	"amount":  "1000000upc",
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
pchaind tx staking create-validator $HOME/validator.json --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Remove Node

```bash
sudo systemctl stop pchaind
sudo systemctl disable pchaind
sudo rm -rf /etc/systemd/system/pchaind.service
sudo rm $(which pchaind)
sudo rm -rf $HOME/.pushchain
```
