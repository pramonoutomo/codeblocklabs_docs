---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful Command

### 🔑 Key management <a href="#key-management" id="key-management"></a>

**Add new key**

```
symphonyd keys add wallet
```

**Recover existing key**

```
symphonyd keys add wallet --recover
```

**List all keys**

```
symphonyd keys list
```

**Delete key**

```
symphonyd keys delete wallet
```

**Export key to a file**

```
symphonyd keys export wallet
```

**Import key from a file**

```
symphonyd keys import wallet wallet.backup
```

**Query wallet balance**

```
symphonyd q bank balances $(symphonyd keys show wallet -a)
```

### 👷 Validator management <a href="#validator-management" id="validator-management"></a>

Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.

**Create new validator**

```
lumerad tx staking create-validator <(cat <<EOF
{
  "pubkey": $(symphonyd comet show-validator),
  "amount": "1000000note",
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
--chain-id symphony-testnet-4 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025note \
-y
```

**Edit existing validator**

```
symphonyd tx staking edit-validator \
--new-moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id symphony-testnet-4 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025note \
-y
```

**Unjail validator**

```
symphonyd tx slashing unjail --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Jail reason**

```
symphonyd query slashing signing-info $(symphonyd comet show-validator)
```

**List all active validators**

```
symphonyd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**List all inactive validators**

```
symphonyd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**View validator details**

```
symphonyd q staking validator $(symphonyd keys show wallet --bech val -a)
```

### 💲 Token management <a href="#token-management" id="token-management"></a>

**Withdraw rewards from all validators**

```
symphonyd tx distribution withdraw-all-rewards --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Withdraw commission and rewards from your validator**

```
symphonyd tx distribution withdraw-rewards $(symphonyd keys show wallet --bech val -a) --commission --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Delegate tokens to yourself**

```
symphonyd tx staking delegate $(symphonyd keys show wallet --bech val -a) 1000000note --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Delegate tokens to validator**

```
symphonyd tx staking delegate <TO_VALOPER_ADDRESS> 1000000note --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Redelegate tokens to another validator**

```
symphonyd tx staking redelegate $(symphonyd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000note --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Unbond tokens from your validator**

```
symphonyd tx staking unbond $(symphonyd keys show wallet --bech val -a) 1000000note --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Send tokens to the wallet**

```
symphonyd tx bank send wallet <TO_WALLET_ADDRESS> 1000000note --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

### 🗳 Governance <a href="#governance" id="governance"></a>

**List all proposals**

```
symphonyd query gov proposals
```

**View proposal by id**

```
symphonyd query gov proposal 1
```

**Vote ‘Yes’**

```
symphonyd tx gov vote 1 yes --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Vote ‘No’**

```
symphonyd tx gov vote 1 no --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Vote ‘Abstain’**

```
symphonyd tx gov vote 1 abstain --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

**Vote ‘NoWithVeto’**

```
symphonyd tx gov vote 1 NoWithVeto --from wallet --chain-id symphony-testnet-4 --gas-adjustment 1.4 --gas auto --gas-prices 0.025note -y
```

### ⚡️ Utility <a href="#utility" id="utility"></a>

**Update ports**

```
CUSTOM_PORT=110
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.symphonyd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.symphonyd/config/app.toml
```

**Update Indexer**

**Disable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.symphonyd/config/config.toml
```

**Enable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.symphonyd/config/config.toml
```

**Update pruning**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.symphonyd/config/app.toml
```

### 🚨 Maintenance <a href="#maintenance" id="maintenance"></a>

**Get validator info**

```
symphonyd status 2>&1 | jq .ValidatorInfo
```

**Get sync info**

```
symphonyd status 2>&1 | jq .SyncInfo
```

**Get node peer**

```
echo $(symphonyd comet show-node-id)'@'$(curl -4s ifconfig.me)':'$(cat $HOME/.symphonyd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

**Check if validator key is correct**

```
[[ $(symphonyd q staking validator $(symphonyd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(symphonyd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

**Get live peers**

```
curl -sS http://localhost:PORT/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

**Set minimum gas price**

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025ulume\"/" $HOME/.symphonyd/config/app.toml
```

**Enable prometheus**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.symphonyd/config/config.toml
```

**Reset chain data**

```
symphonyd comet unsafe-reset-all --keep-addr-book --home $HOME/.symphonyd --keep-addr-book
```

**Remove node**

Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!

```
cd $HOME
sudo systemctl stop symphony-testnet.service
sudo systemctl disable symphony-testnet.service
sudo rm /etc/systemd/system/symphony-testnet.service
sudo systemctl daemon-reload
rm -f $(which symphonyd)
rm -rf $HOME/.symphonyd
rm -rf $HOME/symphonyd
```

### ⚙️ Service Management <a href="#service-management" id="service-management"></a>

**Reload service configuration**

```
sudo systemctl daemon-reload
```

**Enable service**

```
sudo systemctl enable symphony-testnet.service
```

**Disable service**

```
sudo systemctl disable symphony-testnet.service
```

**Start service**

```
sudo systemctl start symphony-testnet.service
```

**Stop service**

```
sudo systemctl stop symphony-testnet.service
```

**Restart service**

```
sudo systemctl restart symphony-testnet.service
```

**Check service status**

```
sudo systemctl status symphony-testnet.service
```

**Check service logs**

```
sudo journalctl -u symphony-testnet.service -f --no-hostname -o cat
```
