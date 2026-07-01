# Token Management

## Token Management

### Withdraw All Rewards

```bash
limonatad tx distribution withdraw-all-rewards --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Withdraw Rewards and Commission

```bash
limonatad tx distribution withdraw-rewards $(limonatad keys show wallet --bech val -a --home $HOME/.limonata) --from wallet --commission --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Check Balance

```bash
limonatad query bank balances $(limonatad keys show wallet -a --home $HOME/.limonata) --home $HOME/.limonata
```

### Delegate to Yourself

```bash
limonatad tx staking delegate $(limonatad keys show wallet --bech val -a --home $HOME/.limonata) 1000000alimo --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Delegate to Other Validator

```bash
limonatad tx staking delegate VALOPER_ADDRESS 1000000alimo --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Redelegate Stake

```bash
limonatad tx staking redelegate $(limonatad keys show wallet --bech val -a --home $HOME/.limonata) VALOPER_ADDRESS 1000000alimo --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Unbond

```bash
limonatad tx staking unbond $(limonatad keys show wallet --bech val -a --home $HOME/.limonata) 1000000alimo --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Transfer Funds

```bash
limonatad tx bank send $(limonatad keys show wallet -a --home $HOME/.limonata) WALLET_ADDRESS 1000000alimo --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```
