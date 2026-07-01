# Services Management

## Services Management

### Check Logs

```bash
sudo journalctl -u pchaind -f
```

### Sync Info

```bash
pchaind status 2>&1 | jq .sync_info --home $HOME/.pushchain
```

### Node Info

```bash
pchaind status 2>&1 | jq .node_info --home $HOME/.pushchain
```

### Your Node Peer

```bash
echo $(pchaind comet show-node-id --home $HOME/.pushchain)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.pushchain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Restart Node

```bash
sudo systemctl restart pchaind && sudo journalctl -u pchaind -f
```
