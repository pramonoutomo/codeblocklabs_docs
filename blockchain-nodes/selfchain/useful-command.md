---
description: >-
  Useful set of commands for node operators. From key management to chain
  governance.
---

# Useful Command

### 🔑 Key management <a href="#key-management" id="key-management"></a>

**Add new key**

```
selfchaind keys add wallet
```

**Recover existing key**

```
selfchaind keys add wallet --recover
```

**List all keys**

```
selfchaind keys list
```

**Delete key**

```
selfchaind keys delete wallet
```

**Export key to the file**

```
selfchaind keys export wallet
```

**Import key from the file**

```
selfchaind keys import wallet wallet.backup
```

**Query wallet balance**

```
selfchaind q bank balances $(selfchaind keys show wallet -a)
```

### 👷 Validator management <a href="#validator-management" id="validator-management"></a>

Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.

**Create new validator**

```
selfchaind tx staking create-validator \
  --amount "1000000uslf" \
  --pubkey $(selfchaind tendermint show-validator) \
  --moniker "<MONIKER>" \
  --identity "" \
  --details "CodeBlockLabs Community" \
  --website "YOUR WEBSITE" \
  --chain-id selfchain-testnet \
  --commission-rate "0.05" \
  --commission-max-rate "0.2" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1" \
  --gas-prices 1uslf \
  --gas "auto" \
  --gas-adjustment "1.5" \
  --from wallet \
  -y

```

**Edit existing validator**

```
selfchaind tx staking edit-validator \
--new-moniker "<MONIKER>" \
--identity "" \
--details "CodeBlockLabs Community" \
--website "YOUR WEBSITE" \
--chain-id selfchain-testnet \
--commission-rate "0.05" \
--gas-prices 1uslf \
--gas "auto" \
--gas-adjustment "1.5" \
--from wallet \
-y
```

**Unjail validator**

```
selfchaind tx slashing unjail \
--chain-id selfchain-testnet \
--gas-prices 1uslf \
--gas-adjustment 1.5 \
--gas "auto" \
--from wallet \
-y 
```

**Jail reason**

```
selfchaind query slashing signing-info $(selfchaind tendermint show-validator)
```

**List all active validators**

```
selfchaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl 
```

**List all inactive validators**

```
selfchaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED") or .status=="BOND_STATUS_UNBONDING")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " 	 " + .description.moniker' | sort -gr | nl 
```

**View validator details**

```
selfchaind q staking validator $(selfchaind keys show wallet --bech val -a) 
```

### 💲 Token management <a href="#token-management" id="token-management"></a>

**Withdraw rewards from all validators**

```
selfchaind tx distribution withdraw-all-rewards --from wallet --chain-id selfchain-testnet --gas-prices 1uslf  --gas-adjustment 1.5 --gas "auto" -y 
```

**Withdraw commission and rewards from your validator**

```
selfchaind tx distribution withdraw-rewards $(selfchaind keys show wallet --bech val -a) --commission --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Delegate tokens to yourself**

```
selfchaind tx staking delegate $(selfchaind keys show wallet --bech val -a) 1000000uslf --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Delegate tokens to validator**

```
selfchaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000uslf --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Redelegate tokens to another validator**

```
selfchaind tx staking redelegate $(selfchaind keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uslf --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Unbond tokens from your validator**

```
selfchaind tx staking unbond $(selfchaind keys show wallet --bech val -a) 1000000uslf --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Send tokens to the wallet**

```
selfchaind tx bank send wallet <TO_WALLET_ADDRESS> 1000000uslf --from wallet --chain-id selfchain-testnet
```

### 🗳 Governance <a href="#governance" id="governance"></a>

**List all proposals**

```
selfchaind query gov proposals
```

**View proposal by id**

```
selfchaind query gov proposal 1
```

**Vote 'Yes'**

```
selfchaind tx gov vote 1 yes --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Vote 'No'**

```
selfchaind tx gov vote 1 no --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Vote 'Abstain'**

```
selfchaind tx gov vote 1 abstain --from wallet --chain-id selfchain-testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

**Vote 'NoWithVeto'**

```
selfchaind tx gov vote 1 nowithveto --from wallet --chain-id selfchain--testnet --gas-adjustment 1.4 --gas auto --fees 500uslf -y
```

### ⚡️ Utility <a href="#utility" id="utility"></a>

**Update ports**

```
CUSTOM_PORT=20
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.selfchaind/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.selfchaind/config/app.toml
```

## **Update Indexer**

**Disable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.selfchaind/config/config.toml
```

**Enable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.selfchaind/config/config.toml
```

**Update pruning**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.selfchaind/config/app.toml
```

## 🚨 Maintenance <a href="#maintenance" id="maintenance"></a>

**Get validator info**

```
selfchaind status 2>&1 | jq .ValidatorInfo
```

**Get sync info**

```
selfchaind status 2>&1 | jq .SyncInfo
```

**Get node peer**

```
echo $(selfchaind tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.selfchaind/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

**Check if validator key is correct**

```
[[ $(selfchaind q staking validator $(selfchaind keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(blockxd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

**Get live peers**

```
curl -sS http://localhost:25657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

**Set minimum gas price**

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uslf\"/" $HOME/.selfchaind/config/app.toml
```

**Enable prometheus**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.selfchaind/config/config.toml
```

**Reset chain data**

```
selfchaind tendermint unsafe-reset-all --home $HOME/.selfchaind --keep-addr-book
```

**Remove node**

Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!

```
cd $HOME
sudo systemctl stop selfchaind 
sudo systemctl disable selfchaind 
sudo rm /etc/systemd/system/selfchaind.service
sudo systemctl daemon-reload
rm -f $(which selfchaind)
rm -rf $HOME/.selfchaind

```

### ⚙️ Service Management <a href="#service-management" id="service-management"></a>

**Reload service configuration**

```
sudo systemctl daemon-reload
```

**Enable service**

```
sudo systemctl enable selfchaind
```

**Disable service**

```
sudo systemctl disable selfchaind
```

**Start service**

```
sudo systemctl start selfchaind
```

**Stop service**

```
sudo systemctl stop selfchaind
```

**Restart service**

```
sudo systemctl restart selfchaind
```

**Check service status**

```
sudo systemctl status selfchaind
```

**Check service logs**

```
sudo journalctl -u selfchaind -f --no-hostname -o cat
```
