# Validator Management

## Validator Management

### Edit Existing Validator

```bash
latandad tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id latanda-testnet-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd \
--home $HOME/.latanda \
-y
```

### Validator Info

```bash
latandad status 2>&1 | jq .validator_info --home $HOME/.latanda
```

### Validator Details

```bash
latandad q staking validator $(latandad keys show wallet --bech val -a --home $HOME/.latanda) --home $HOME/.latanda
```

### Jailing Info

```bash
latandad q slashing signing-info $(latandad comet show-validator --home $HOME/.latanda) --home $HOME/.latanda
```

### Slashing Parameters

```bash
latandad q slashing params --home $HOME/.latanda
```

### Unjail Validator

```bash
latandad tx slashing unjail --from wallet --chain-id latanda-testnet-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25ultd --home $HOME/.latanda -y
```

### Active Validators List

```bash
latandad q staking validators -oj --limit=2000 --home $HOME/.latanda | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

### Signing Info

```bash
latandad q slashing signing-info $(latandad comet show-validator --home $HOME/.latanda) --home $HOME/.latanda
```
