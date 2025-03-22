---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful Command

### 🔑 Key management <a href="#key-management" id="key-management"></a>

**Add new key**

```
blockxd keys add wallet
```

**Recover existing key**

```
blockxd keys add wallet --recover
```

**List all keys**

```
blockxd keys list
```

**Delete key**

```
blockxd keys delete wallet
```

**Export key to the file**

```
blockxd keys export wallet
```

**Import key from the file**

```
blockxd keys import wallet wallet.backup
```

**Query wallet balance**

```
blockxd q bank balances $(blockxd keys show wallet -a)
```

### 👷 Validator management <a href="#validator-management" id="validator-management"></a>

Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.

**Create new validator**

```
blockxd tx staking create-validator \
--amount 1000000abcx \
--pubkey $(blockxd tendermint show-validator) \
--moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id blockx_12345-2 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-prices 0.025abcx \
-y
```

**Edit existing validator**

```
blockxd tx staking edit-validator \
--moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL"
--chain-id blockx_12345-2 \
--commission-rate 0.05 \
--from wallet \
--gas-prices 0.025abcx \
-y
```

**Unjail validator**

```
blockxd tx slashing unjail --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas-prices 0.025abcx -y
```

**Jail reason**

```
blockxd query slashing signing-info $(blockxd tendermint show-validator)
```

**List all active validators**

```
blockxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**List all inactive validators**

```
blockxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**View validator details**

```
blockxd q staking validator $(blockxd keys show wallet --bech val -a)
```

### 💲 Token management <a href="#token-management" id="token-management"></a>

**Withdraw rewards from all validators**

```
blockxd tx distribution withdraw-all-rewards --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Withdraw commission and rewards from your validator**

```
blockxd tx distribution withdraw-rewards $(blockxd keys show wallet --bech val -a) --commission --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Delegate tokens to yourself**

```
blockxd tx staking delegate $(blockxd keys show wallet --bech val -a) 1000000abcx --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Delegate tokens to validator**

```
blockxd tx staking delegate <TO_VALOPER_ADDRESS> 1000000abcx --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Redelegate tokens to another validator**

```
blockxd tx staking redelegate $(blockxd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000abcx --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Unbond tokens from your validator**

```
blockxd tx staking unbond $(blockxd keys show wallet --bech val -a) 1000000abcx --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Send tokens to the wallet**

```
blockxd tx bank send wallet <TO_WALLET_ADDRESS> 1000000abcx --from wallet --chain-id blockx_12345-2
```

### 🗳 Governance <a href="#governance" id="governance"></a>

**List all proposals**

```
blockxd  query gov proposals
```

**View proposal by id**

```
blockxd query gov proposal 1
```

**Vote 'Yes'**

```
blockxd tx gov vote 1 yes --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Vote 'No'**

```
blockxd tx gov vote 1 no --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Vote 'Abstain'**

```
blockxd tx gov vote 1 abstain --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

**Vote 'NoWithVeto'**

```
blockxd tx gov vote 1 nowithveto --from wallet --chain-id blockx_12345-2 --gas-adjustment 1.4 --gas auto --fees 500abcx -y
```

### ⚡️ Utility <a href="#utility" id="utility"></a>

**Update ports**

```
CUSTOM_PORT=28
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.blockxd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.nolus/config/app.toml
```

## **Update Indexer**

**Disable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.blockxd/config/config.toml
```

**Enable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.blockxd/config/config.toml
```

**Update pruning**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.blockxd/config/app.toml
```

## 🚨 Maintenance <a href="#maintenance" id="maintenance"></a>

**Get validator info**

```
blockxd status 2>&1 | jq .ValidatorInfo
```

**Get sync info**

```
blockxd status 2>&1 | jq .SyncInfo
```

**Get node peer**

```
echo $(blockxd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.blockxd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

**Check if validator key is correct**

```
[[ $(blockxd q staking validator $(blockxd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(blockxd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

**Get live peers**

```
curl -sS http://localhost:25657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

**Set minimum gas price**

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025unls\"/" $HOME/.blockxd/config/app.toml
```

**Enable prometheus**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.blockxd/config/config.toml
```

**Reset chain data**

```
blockxd tendermint unsafe-reset-all --home $HOME/.blockxd --keep-addr-book
```

**Remove node**

Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!

```
cd $HOME
sudo systemctl stop blockxd 
sudo systemctl disable blockxd 
sudo rm /etc/systemd/system/blockxd.service
sudo systemctl daemon-reload
rm -f $(which blockxd)
rm -rf $HOME/.blockxd
rm -rf $HOME/blockx-node-public-compiled
```

### ⚙️ Service Management <a href="#service-management" id="service-management"></a>

**Reload service configuration**

```
sudo systemctl daemon-reload
```

**Enable service**

```
sudo systemctl enable blockx
```

**Disable service**

```
sudo systemctl disable blockx
```

**Start service**

```
sudo systemctl start blockx
```

**Stop service**

```
sudo systemctl stop blockx
```

**Restart service**

```
sudo systemctl restart blockx
```

**Check service status**

```
sudo systemctl status blockx
```

**Check service logs**

```
sudo journalctl -u blockx -f --no-hostname -o cat
```
