# Cheatshee

### ⚙️Service operations <a href="#service-operations" id="service-operations"></a>

Check logs

```
sudo journalctl -fu emped
```

Reload daemon

```
sudo systemctl daemon-reload
```

Enable service

```
sudo systemctl enable emped
```

Disable service

```
sudo systemctl disable emped
```

Start service

```
sudo systemctl start emped
```

Stop service

```
sudo systemctl stop emped
```

Restart service

```
sudo systemctl status emped
```

Check service status

```
sudo systemctl status emped
```

### 🖥️Node operations <a href="#node-operations" id="node-operations"></a>

Overall statusNode infoSync info

```
emped status | jq
```

Node info

```
emped status 2>&1 | jq .NodeInfo
```

Sync info

```
emped status 2>&1 | jq .SyncInfo
```

### 🗝️Key Management <a href="#key-management" id="key-management"></a>

Add new

```
emped keys add $WALLET
```

Restore wallet

```
emped keys add $WALLET --recover
```

List all wallet

```
emped keys list
```

Check balance

```
emped q bank balances $(emped keys show $WALLET -a)
```

### 💱Transaction operations <a href="#transaction-operations" id="transaction-operations"></a>

Withdraw all rewards

```
emped tx distribution withdraw-all-rewards \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees 20uempe -y
```

Withdraw rewards and commission from your validator

```
emped tx distribution withdraw-rewards $(emped keys show $WALLET --bech val -a) \
--from $WALLET \
--commission \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees 20uempe -y
```

Self delegate

```
emped tx staking delegate $(emped keys show $WALLET --bech val -a) 1000000uempe \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees 20uempe -y
```

Redelegate

```
emped tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000uempe \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees 20uempe -y
```

Unbond

```
emped tx staking unbond $(emped keys show $WALLET --bech val -a) 1000000uempe \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto --gas-adjustment 1.5 \
--fees 20uempe -y
```

Transfer token

```
emped tx bank send $WALLET <TO_WALLET_ADDRESS> 1000000uempe \
--gas auto \
--gas-adjustment 1.5 \
--fees 20uempe -y
```

### ✅Validator operations <a href="#validator-operations" id="validator-operations"></a>

Unjail Validator

```
emped tx slashing unjail \
--from $WALLET \
--chain-id empe-testnet-2 \
--fees=20uempe -y
```

### 🏛️Governance <a href="#governance" id="governance"></a>

Query Proposal List

```
emped query gov proposals
```

Vote

```
emped tx gov vote 1 yes \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees=20uempe -y
```

vote value can be `yes`, `no`, `no_with_veto` and `abstain`
