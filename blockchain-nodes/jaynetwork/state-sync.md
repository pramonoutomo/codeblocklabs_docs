# State Sync

```plaintext
sudo systemctl stop jaynd
```

```plaintext
cp $HOME/.jayn/data/priv_validator_state.json $HOME/.jayn/priv_validator_state.json.backup
```

```plaintext
jaynd comet unsafe-reset-all --home $HOME/.jayn --keep-addr-book
```

```plaintext
SNAP_RPC="http://152.53.195.5:26657"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000))
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
sed -i \
-e "s|^enable *=.*|enable = true|" \
-e "s|^rpc_servers *=.*|rpc_servers = \"$SNAP_RPC,$SNAP_RPC\"|" \
-e "s|^trust_height *=.*|trust_height = $BLOCK_HEIGHT|" \
-e "s|^trust_hash *=.*|trust_hash = \"$TRUST_HASH\"|" \
$HOME/.jayn/config/config.toml
```

```plaintext
mv $HOME/.jayn/priv_validator_state.json.backup $HOME/.jayn/data/priv_validator_state.json
```

```plaintext
sudo systemctl restart jaynd && sudo journalctl -u jaynd -f
```
