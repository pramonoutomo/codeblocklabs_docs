---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful Command

### 🔑 Key management <a href="#key-management" id="key-management"></a>

**Add new key**

```
lumerad keys add wallet
```

**Recover existing key**

```
lumerad keys add wallet --recover
```

**List all keys**

```
lumerad keys list
```

**Delete key**

```
lumerad keys delete wallet
```

**Export key to a file**

```
lumerad keys export wallet
```

**Import key from a file**

```
lumerad keys import wallet wallet.backup
```

**Query wallet balance**

```
lumerad q bank balances $(lumerad keys show wallet -a)
```

### 👷 Validator management <a href="#validator-management" id="validator-management"></a>

Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.

**Create new validator**

```
lumerad tx staking create-validator <(cat <<EOF
{
  "pubkey": $(lumerad comet show-validator),
  "amount": "1000000ulume",
  "moniker": "YOUR_MONIKER_NAME",
  "identity": "YOUR_KEYBASE_ID",
  "website": "YOUR_WEBSITE_URL",
  "security": "YOUR_SECURITY_EMAIL",
  "details": "YOUR_DETAILS",
  "commission-rate": "0.05",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.05",
  "min-self-delegation": "1"
}
EOF
) \
--chain-id lumera-testnet-1 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025ulume \
-y
```

**Edit existing validator**

```
lumerad tx staking edit-validator \
--new-moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id lumera-testnet-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025ulume \
-y
```

**Unjail validator**

```
lumerad tx slashing unjail --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Jail reason**

```
lumerad query slashing signing-info $(lumerad comet show-validator)
```

**List all active validators**

```
lumerad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**List all inactive validators**

```
lumerad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**View validator details**

```
lumerad q staking validator $(lumerad keys show wallet --bech val -a)
```

### 💲 Token management <a href="#token-management" id="token-management"></a>

**Withdraw rewards from all validators**

```
lumerad tx distribution withdraw-all-rewards --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Withdraw commission and rewards from your validator**

```
lumerad tx distribution withdraw-rewards $(lumerad keys show wallet --bech val -a) --commission --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Delegate tokens to yourself**

```
lumerad tx staking delegate $(lumerad keys show wallet --bech val -a) 1000000ulume --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Delegate tokens to validator**

```
lumerad tx staking delegate <TO_VALOPER_ADDRESS> 1000000ulume --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Redelegate tokens to another validator**

```
lumerad tx staking redelegate $(lumerad keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ulume --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Unbond tokens from your validator**

```
lumerad tx staking unbond $(lumerad keys show wallet --bech val -a) 1000000ulume --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Send tokens to the wallet**

```
lumerad tx bank send wallet <TO_WALLET_ADDRESS> 1000000ulume --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

### 🗳 Governance <a href="#governance" id="governance"></a>

**List all proposals**

```
lumerad query gov proposals
```

**View proposal by id**

```
lumerad query gov proposal 1
```

**Vote ‘Yes’**

```
lumerad tx gov vote 1 yes --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Vote ‘No’**

```
lumerad tx gov vote 1 no --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Vote ‘Abstain’**

```
lumerad tx gov vote 1 abstain --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

**Vote ‘NoWithVeto’**

```
lumerad tx gov vote 1 NoWithVeto --from wallet --chain-id lumera-testnet-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025ulume -y
```

### ⚡️ Utility <a href="#utility" id="utility"></a>

**Update ports**

```
CUSTOM_PORT=110
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.lumera/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.lumera/config/app.toml
```

**Update Indexer**

**Disable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.lumera/config/config.toml
```

**Enable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.lumera/config/config.toml
```

**Update pruning**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.lumera/config/app.toml
```

### 🚨 Maintenance <a href="#maintenance" id="maintenance"></a>

**Get validator info**

```
lumerad status 2>&1 | jq .ValidatorInfo
```

**Get sync info**

```
lumerad status 2>&1 | jq .SyncInfo
```

**Get node peer**

```
echo $(lumerad comet show-node-id)'@'$(curl -4s ifconfig.me)':'$(cat $HOME/.lumera/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

**Check if validator key is correct**

```
[[ $(lumerad q staking validator $(lumerad keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(lumerad status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

**Get live peers**

```
curl -sS http://localhost:16957/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

**Set minimum gas price**

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025ulume\"/" $HOME/.lumera/config/app.toml
```

**Enable prometheus**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.lumera/config/config.toml
```

**Reset chain data**

```
lumerad comet unsafe-reset-all --keep-addr-book --home $HOME/.lumera --keep-addr-book
```

**Remove node**

Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!

```
cd $HOME
sudo systemctl stop lumera.service
sudo systemctl disable lumera.service
sudo rm /etc/systemd/system/lumera.service
sudo systemctl daemon-reload
rm -f $(which lumerad)
rm -rf $HOME/.lumera
rm -rf $HOME/lumera
```

### ⚙️ Service Management <a href="#service-management" id="service-management"></a>

**Reload service configuration**

```
sudo systemctl daemon-reload
```

**Enable service**

```
sudo systemctl enable lumera.service
```

**Disable service**

```
sudo systemctl disable lumera.service
```

**Start service**

```
sudo systemctl start lumera.service
```

**Stop service**

```
sudo systemctl stop lumera.service
```

**Restart service**

```
sudo systemctl restart lumera.service
```

**Check service status**

```
sudo systemctl status lumera.service
```

**Check service logs**

```
sudo journalctl -u lumera.service -f --no-hostname -o cat
```
