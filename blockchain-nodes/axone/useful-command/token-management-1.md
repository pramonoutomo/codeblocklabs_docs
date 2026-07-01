# Token Management

## Token Management

### Withdraw All Rewards

```bash
pchaind tx distribution withdraw-all-rewards --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Withdraw Rewards and Commission

```bash
pchaind tx distribution withdraw-rewards $(pchaind keys show wallet --bech val -a --home $HOME/.pushchain) --from wallet --commission --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Check Balance

```bash
pchaind query bank balances $(pchaind keys show wallet -a --home $HOME/.pushchain) --home $HOME/.pushchain
```

### Delegate to Yourself

```bash
pchaind tx staking delegate $(pchaind keys show wallet --bech val -a --home $HOME/.pushchain) 1000000upc --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Delegate to Other Validator

```bash
pchaind tx staking delegate VALOPER_ADDRESS 1000000upc --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Redelegate Stake

```bash
pchaind tx staking redelegate $(pchaind keys show wallet --bech val -a --home $HOME/.pushchain) VALOPER_ADDRESS 1000000upc --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Unbond

```bash
pchaind tx staking unbond $(pchaind keys show wallet --bech val -a --home $HOME/.pushchain) 1000000upc --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Transfer Funds

```bash
pchaind tx bank send $(pchaind keys show wallet -a --home $HOME/.pushchain) WALLET_ADDRESS 1000000upc --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```
