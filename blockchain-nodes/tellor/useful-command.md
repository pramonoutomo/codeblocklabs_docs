# Useful Command

#### 🔑 Key management <a href="#key-management" id="key-management"></a>

**Add new key**

```
layerd keys add wallet
```

**Recover existing key**

```
layerd keys add wallet --recover
```

**List all keys**

```
layerd keys list
```

**Delete key**

```
layerd keys delete wallet
```

**Export key to a file**

```
layerd keys export wallet
```

**Import key from a file**

```
layerd keys import wallet wallet.backup
```

**Query wallet balance**

```
layerd q bank balances $(layerd keys show wallet -a)
```

#### 👷 Validator management <a href="#validator-management" id="validator-management"></a>

Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.

**Create new validator**

```
layerd tx staking create-validator <(cat <<EOF
{
  "pubkey": $(layerd comet show-validator),
  "amount": "1000000loya",
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
--chain-id tellor-1 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025loya \
-y
```

**Edit existing validator**

```
layerd tx staking edit-validator \
--new-moniker "YOUR_MONIKER_NAME" \
--identity "YOUR_KEYBASE_ID" \
--details "YOUR_DETAILS" \
--website "YOUR_WEBSITE_URL" \
--chain-id tellor-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.025loya \
-y
```

**Unjail validator**

```
layerd tx slashing unjail --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Jail reason**

```
layerd query slashing signing-info $(layerd comet show-validator)
```

**List all active validators**

```
layerd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**List all inactive validators**

```
layerd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**View validator details**

```
layerd q staking validator $(layerd keys show wallet --bech val -a)
```

#### 💲 Token management <a href="#token-management" id="token-management"></a>

**Withdraw rewards from all validators**

```
layerd tx distribution withdraw-all-rewards --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Withdraw commission and rewards from your validator**

```
layerd tx distribution withdraw-rewards $(layerd keys show wallet --bech val -a) --commission --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Delegate tokens to yourself**

```
layerd tx staking delegate $(layerd keys show wallet --bech val -a) 1000000loya --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Delegate tokens to validator**

```
layerd tx staking delegate <TO_VALOPER_ADDRESS> 1000000loya --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Redelegate tokens to another validator**

```
layerd tx staking redelegate $(layerd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000loya --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Unbond tokens from your validator**

```
layerd tx staking unbond $(layerd keys show wallet --bech val -a) 1000000loya --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Send tokens to the wallet**

```
layerd tx bank send wallet <TO_WALLET_ADDRESS> 1000000loya --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

#### 🗳 Governance <a href="#governance" id="governance"></a>

**List all proposals**

```
layerd query gov proposals
```

**View proposal by id**

```
layerd query gov proposal 1
```

**Vote ‘Yes’**

```
layerd tx gov vote 1 yes --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Vote ‘No’**

```
layerd tx gov vote 1 no --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Vote ‘Abstain’**

```
layerd tx gov vote 1 abstain --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

**Vote ‘NoWithVeto’**

```
layerd tx gov vote 1 NoWithVeto --from wallet --chain-id tellor-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025loya -y
```

#### ⚡️ Utility <a href="#utility" id="utility"></a>

**Update ports**

```
CUSTOM_PORT=110
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.layer/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.layer/config/app.toml
```

**Update Indexer**

**Disable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.layer/config/config.toml
```

**Enable indexer**

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.layer/config/config.toml
```

**Update pruning**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.layer/config/app.toml
```

#### 🚨 Maintenance <a href="#maintenance" id="maintenance"></a>

**Get validator info**

```
layerd status 2>&1 | jq .ValidatorInfo
```

**Get sync info**

```
layerd status 2>&1 | jq .SyncInfo
```

**Get node peer**

```
echo $(layerd comet show-node-id)'@'$(curl -4s ifconfig.me)':'$(cat $HOME/.layer/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

**Check if validator key is correct**

```
[[ $(layerd rad q staking validator $(layerd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(layerd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

**Get live peers**

```
curl -sS http://localhost:16957/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

**Set minimum gas price**

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025loya\"/" $HOME/.layer/config/app.toml
```

**Enable prometheus**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.layer/config/config.toml
```

**Reset chain data**

```
layerd comet unsafe-reset-all --keep-addr-book --home $HOME/.layer --keep-addr-book
```

**Remove node**

Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv\_validator\_key.json**!

```
cd $HOME
sudo systemctl stop layerd.service
sudo systemctl disable layerd.service
sudo rm /etc/systemd/system/layerd.service
sudo systemctl daemon-reload
rm -f $(which layerd)
rm -rf $HOME/.layerd
rm -rf $HOME/layerd
```

#### ⚙️ Service Management <a href="#service-management" id="service-management"></a>

**Reload service configuration**

```
sudo systemctl daemon-reload
```

**Enable service**

```
sudo systemctl enable layerd.service
```

**Disable service**

```
sudo systemctl disable layerd.service
```

**Start service**

```
sudo systemctl start layerd.service
```

**Stop service**

```
sudo systemctl stop layerd.service
```

**Restart service**

```
sudo systemctl restart layerd.service
```

**Check service status**

```
sudo systemctl status layerd.service
```

**Check service logs**

```
sudo journalctl -u layerd.service -f --no-hostname -o cat
```
