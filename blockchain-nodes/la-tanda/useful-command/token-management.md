# Token Management

## Token Management

### Withdraw All Rewards

```bash
latandad tx distribution withdraw-all-rewards --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Withdraw Rewards and Commission

```bash
latandad tx distribution withdraw-rewards $(latandad keys show wallet --bech val -a --home $HOME/.latanda) --from wallet --commission --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Check Balance

```bash
latandad query bank balances $(latandad keys show wallet -a --home $HOME/.latanda) --home $HOME/.latanda
```

### Delegate to Yourself

```bash
latandad tx staking delegate $(latandad keys show wallet --bech val -a --home $HOME/.latanda) 1000000ultd --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Delegate to Other Validator

```bash
latandad tx staking delegate VALOPER_ADDRESS 1000000ultd --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Redelegate Stake

```bash
latandad tx staking redelegate $(latandad keys show wallet --bech val -a --home $HOME/.latanda) VALOPER_ADDRESS 1000000ultd --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Unbond

```bash
latandad tx staking unbond $(latandad keys show wallet --bech val -a --home $HOME/.latanda) 1000000ultd --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Transfer Funds

```bash
latandad tx bank send $(latandad keys show wallet -a --home $HOME/.latanda) WALLET_ADDRESS 1000000ultd --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```
