# Validator Management

Edit Existing Validator

```bash
republicd tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "YOUR_NODES_NAME" \
--identity "YOUR_KEYBASE" \
--details "YOUR_DETAIL" \
--from wallet \
--chain-id raitestnet_77701-1 \
--gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai \
-y
```

Validator info

```bash
republicd status 2>&1 | jq .validator_info
```

Validator Details

```bash
republicd q staking validator $(republicd keys show wallet --bech val -a)
```

Jailing info

```bash
republicd q slashing signing-info $(republicd comet show-validator)
```

Slashing parameters

```bash
republicd q slashing params
```

Unjail validator

```bash
republicd tx slashing unjail --from wallet --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5  --gas-prices 2500000000arai -y
```

Active Validators List

```bash
republicd q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl
```

Check Validator key

```bash
[[ $(republicd q staking validator $(republicd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(republicd status 2>&1 | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

Signing info

```bash
republicd q slashing signing-info $(republicd comet show-validator)
```
