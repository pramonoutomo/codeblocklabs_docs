# Token Management

## Token Management

### Withdraw All Rewards

```bash
qied tx distribution withdraw-all-rewards --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Withdraw Rewards and Commission

```bash
qied tx distribution withdraw-rewards $(qied keys show wallet --bech val -a --home $HOME/.qie) --from wallet --commission --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Check Balance

```bash
qied query bank balances $(qied keys show wallet -a --home $HOME/.qie) --home $HOME/.qie
```

### Delegate to Yourself

```bash
qied tx staking delegate $(qied keys show wallet --bech val -a --home $HOME/.qie) 1000000aqie --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Delegate to Other Validator

```bash
qied tx staking delegate VALOPER_ADDRESS 1000000aqie --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Redelegate Stake

```bash
qied tx staking redelegate $(qied keys show wallet --bech val -a --home $HOME/.qie) VALOPER_ADDRESS 1000000aqie --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Unbond

```bash
qied tx staking unbond $(qied keys show wallet --bech val -a --home $HOME/.qie) 1000000aqie --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Transfer Funds

```bash
qied tx bank send $(qied keys show wallet -a --home $HOME/.qie) WALLET_ADDRESS 1000000aqie --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```
