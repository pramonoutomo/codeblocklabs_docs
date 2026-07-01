# Validator Management

## Validator Management

### Edit Existing Validator

```bash
pchaind tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id push_42101-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25upc \
--home $HOME/.pushchain \
-y
```

### Validator Info

```bash
pchaind status 2>&1 | jq .validator_info --home $HOME/.pushchain
```

### Validator Details

```bash
pchaind q staking validator $(pchaind keys show wallet --bech val -a --home $HOME/.pushchain) --home $HOME/.pushchain
```

### Jailing Info

```bash
pchaind q slashing signing-info $(pchaind comet show-validator --home $HOME/.pushchain) --home $HOME/.pushchain
```

### Slashing Parameters

```bash
pchaind q slashing params --home $HOME/.pushchain
```

### Unjail Validator

```bash
pchaind tx slashing unjail --from wallet --chain-id push_42101-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25upc --home $HOME/.pushchain -y
```

### Active Validators List

```bash
pchaind q staking validators -oj --limit=2000 --home $HOME/.pushchain | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

### Signing Info

```bash
pchaind q slashing signing-info $(pchaind comet show-validator --home $HOME/.pushchain) --home $HOME/.pushchain
```
