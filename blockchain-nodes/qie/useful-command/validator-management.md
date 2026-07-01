# Validator Management

## Validator Management

### Edit Existing Validator

```bash
qied tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id qie_1990-1 \
--gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie \
--home $HOME/.qie \
-y
```

### Validator Info

```bash
qied status 2>&1 | jq .validator_info --home $HOME/.qie
```

### Validator Details

```bash
qied q staking validator $(qied keys show wallet --bech val -a --home $HOME/.qie) --home $HOME/.qie
```

### Jailing Info

```bash
qied q slashing signing-info $(qied comet show-validator --home $HOME/.qie) --home $HOME/.qie
```

### Slashing Parameters

```bash
qied q slashing params --home $HOME/.qie
```

### Unjail Validator

```bash
qied tx slashing unjail --from wallet --chain-id qie_1990-1 --gas auto --gas-adjustment 1.5 --gas-prices 0.25aqie --home $HOME/.qie -y
```

### Active Validators List

```bash
qied q staking validators -oj --limit=2000 --home $HOME/.qie | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

### Signing Info

```bash
qied q slashing signing-info $(qied comet show-validator --home $HOME/.qie) --home $HOME/.qie
```
