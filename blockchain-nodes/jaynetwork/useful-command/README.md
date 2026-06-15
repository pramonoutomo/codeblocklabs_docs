# Useful Command

Wallet

```bash
# to create a new wallet, use the following command. don’t forget to save the mnemonic
republicd keys add wallet

# to restore exexuting wallet, use the following command
republicd keys add wallet --recover

# check sync status, once your node is fully synced, the output from above will print "false"
republicd status 2>&1 | jq .sync_info
```

Create Validator File

```bash
# Write your validator configuration.
tee $HOME/validator.json > /dev/null << EOF
{
	"pubkey": $(republicd comet show-validator),
	"amount":  "1000000arai",
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

Deploy Validator From File

```bash
republicd tx staking create-validator $HOME/validator.json --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

<br>

Remove Node

```bash
sudo systemctl stop republicd
sudo systemctl disable republicd
sudo rm -rf /etc/systemd/system/republicd.service
sudo rm $(which republicd)
sudo rm -rf $HOME/.republic
```
