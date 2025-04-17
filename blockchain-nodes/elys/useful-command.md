---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful Command

### Key Management <a href="#key" id="key"></a>

Add new key

```bash
elysd keys add wallet
```

Recover existing key

```bash
elysd keys add wallet --recover
```

List All key

```bash
elysd keys list
```

Delete key

```bash
elysd keys delete wallet
```

Export Key (save to wallet.backup)

```bash
elysd keys export wallet
```

Import key

```bash
elysd keys import wallet wallet.backup
```

Query Wallet Balance

```bash
elysd q bank balances $(elysd keys show wallet -a)
```

***

### Validator Management <a href="#validator" id="validator"></a>

Create Validator

```bash
elysd tx staking create-validator \
  --amount "1000000uelys" \
  --pubkey $(elysd tendermint show-validator) \
  --moniker "My Node Name" \
  --identity "My Keybase ID" \
  --details "My Node Details" \
  --website "YOUR WEBSITE" \
  --chain-id elys-1 \
  --commission-rate "0.05" \
  --commission-max-rate "0.2" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1" \
  --gas-prices 0.0003uelys \
  --gas "auto" \
  --gas-adjustment "1.5" \
  --from wallet \
  -y
```

Edit Validator

```bash
elysd tx staking edit-validator \
--new-moniker "My Node Name" \
--identity "My Keybase ID" \
--details "My Node Details" \
--website "YOUR WEBSITE" \
--chain-id elys-1 \
--commission-rate "0.05" \
--gas-prices 0.0003uelys \
--gas "auto" \
--gas-adjustment "1.5" \
--from wallet \
-y
```

Unjail Validator

```bash
elysd tx slashing unjail \
--chain-id elys-1 \
--gas-prices 0.0003uelys \
--gas-adjustment 1.5 \
--gas "auto" \
--from wallet \
-y 
```

Signing Info

```bash
elysd query slashing signing-info $(elysd tendermint show-validator) 
```

List all inactive validators

```bash
elysd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl 
```

List all active validators

```bash
elysd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED") or .status=="BOND_STATUS_UNBONDING")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl 
```

View validators details

```bash
elysd q staking validator $(elysd keys show wallet --bech val -a) 

```

***

### Token Management <a href="#token" id="token"></a>

Withdraw rewards from all validators

```bash
elysd tx distribution withdraw-all-rewards --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Withdraw comission and rewards from your validator

```bash
elysd tx distribution withdraw-rewards $(elysd keys show wallet --bech val -a) --commission --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Delegate to your validator

```bash
elysd tx staking delegate $(elysd keys show wallet --bech val -a) 1000000uelys --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Delegate to other

```bash
elysd tx staking delegate TO_VALOPER_ADDRESS 1000000uelys --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Redelegate your stake to other validators

```bash
elysd tx staking redelegate $(elysd keys show wallet --bech val -a) TO_VALOPER_ADDRESS 1000000uelys --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Unbond stake

```bash
elysd tx staking unbond $(elysd keys show wallet --bech val -a) 1000000uelys --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

Send tokens

```bash
elysd tx bank send wallet TO_WALLET_ADDREESS 1000000uelys --from wallet --chain-id elys-1 --gas-prices 0.0003uelys  --gas-adjustment 1.5 --gas "auto" -y 
```

### Governance <a href="#governance" id="governance"></a>

Create new text proposal

```bash
elysd tx gov submit-proposal \
--title "New Prosposals" \
--description "Detailed Proposal Information" \
--deposit "1000000uelys" \
--type "Text" \
--from wallet \
--gas-prices 0.0003uelys \ 
--gas-adjustment 1.5 \
--gas "auto" \
-y 
```

List all proposals

```bash
elysd query gov proposals
```

Vote

```bash
elysd tx gov vote PROPOSAL_NUMBER yes \
--from wallet \
--chain-id elys-1 \
--gas-prices 0.0003uelys \
--gas-adjustment 1.5 \
--gas "auto" \
-y 
```

***

### Utility <a href="#utility" id="utility"></a>

Set Indexer to NULL

```bash
sed -i 's|^indexer *=.*|indexer = "null"|' $HOME/.elys/config/config.toml
```

Set Custom Port

```bash
CUSTOM_PORT=13
sed -i.bak -e "s%^proxy_app = "tcp://127.0.0.1:26658"%proxy_app = "tcp://127.0.0.1:${CUSTOM_PORT}658"%; s%^laddr = "tcp://127.0.0.1:26657"%laddr = "tcp://127.0.0.1:${CUSTOM_PORT}657"%; s%^pprof_laddr = "localhost:6060"%pprof_laddr = "localhost:${CUSTOM_PORT}060"%; s%^laddr = "tcp://0.0.0.0:26656"%laddr = "tcp://0.0.0.0:${CUSTOM_PORT}656"%; s%^prometheus_listen_addr = ":26660"%prometheus_listen_addr = ":${CUSTOM_PORT}660"%" $HOME/.elys/config/config.toml
sed -i.bak -e "s%^address = "tcp://0.0.0.0:1317"%address = "tcp://0.0.0.0:${CUSTOM_PORT}317"%; s%^address = ":8080"%address = ":${CUSTOM_PORT}080"%; s%^address = "0.0.0.0:9090"%address = "0.0.0.0:${CUSTOM_PORT}090"%; s%^address = "0.0.0.0:9091"%address = "0.0.0.0:${CUSTOM_PORT}091"%; s%^address = "0.0.0.0:8545"%address = "0.0.0.0:${CUSTOM_PORT}545"%; s%^ws-address = "0.0.0.0:8546"%ws-address = "0.0.0.0:${CUSTOM_PORT}546"%" $HOME/.elys/config/app.toml
```

Get Validator info

```bash
elysd status 2>&1 | jq .ValidatorInfo
```

Get denom info

```bash
elysd q bank denom-metadata -oj | jq
```

Get sync status

```bash
elysd status 2>&1 | jq .SyncInfo.catching_up
```

Get latest height

```bash
elysd status 2>&1 | jq .SyncInfo.latest_block_height
```

Reset Node

```bash
elysd tendermint unsafe-reset-all --home $HOME/.elys --keep-addr-book
```

Delete Node

```bash
cd $HOME && sudo systemctl stop elysd && sudo systemctl disable elysd && sudo rm /etc/systemd/system/elysd.service && sudo systemctl daemon-reload && sudo rm -rf $(which elysd) && sudo rm -rf $HOME/.elys && sudo rm -rf $(which elysd) 
```

***

### Services Management <a href="#services" id="services"></a>

Reload Service

```bash
sudo systemctl daemon-reload
```

Enable Service

```bash
sudo systemctl enable elysd
```

Disable Service

```bash
sudo systemctl disable elysd
```

Start Service

```bash
sudo systemctl start elysd
```

Stop Service

```bash
sudo systemctl stop elysd
```

Restart Service

```bash
sudo systemctl restart elysd
```

Check Service Status

```bash
sudo systemctl status elysd
```

Check Service Logs

```bash
sudo journalctl -u elysd -f --no-hostname -o cat
```
