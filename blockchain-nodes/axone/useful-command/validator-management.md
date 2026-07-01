# Validator Management

## Validator Management

### Edit Existing Validator

```bash
limonatad tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id limonata_10777-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo \
--home $HOME/.limonata \
-y
```

### Validator Info

```bash
limonatad status 2>&1 | jq .validator_info --home $HOME/.limonata
```

### Validator Details

```bash
limonatad q staking validator $(limonatad keys show wallet --bech val -a --home $HOME/.limonata) --home $HOME/.limonata
```

### Jailing Info

```bash
limonatad q slashing signing-info $(limonatad comet show-validator --home $HOME/.limonata) --home $HOME/.limonata
```

### Slashing Parameters

```bash
limonatad q slashing params --home $HOME/.limonata
```

### Unjail Validator

```bash
limonatad tx slashing unjail --from wallet --chain-id limonata_10777-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25alimo --home $HOME/.limonata -y
```

### Active Validators List

```bash
limonatad q staking validators -oj --limit=2000 --home $HOME/.limonata | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

### Signing Info

```bash
limonatad q slashing signing-info $(limonatad comet show-validator --home $HOME/.limonata) --home $HOME/.limonata
```
