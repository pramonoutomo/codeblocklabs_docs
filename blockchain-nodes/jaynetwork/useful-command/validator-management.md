# Validator Management

Edit Existing Validator

```bash
jaynd tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id thejaynetwork \
--gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay \
-y
```

Validator info

```bash
jaynd status 2>&1 | jq .validator_info
```

Validator Details

```bash
jaynd q staking validator $(jaynd keys show wallet --bech val -a)
```

Jailing info

```bash
jaynd q slashing signing-info $(jaynd comet show-validator)
```

Slashing parameters

```bash
jaynd q slashing params
```

Unjail validator

```bash
jaynd tx slashing unjail --from wallet --chain-id thejaynetwork --gas auto --gas-adjustment 1.5  --gas-prices 0.025ujay -y
```

Active Validators List

```bash
jaynd q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl
```

Check Validator key

```bash
[[ $(jaynd q staking validator $(jaynd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(republicd status 2>&1 | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

Signing info

```bash
jaynd q slashing signing-info $(jaynd comet show-validator)
```
