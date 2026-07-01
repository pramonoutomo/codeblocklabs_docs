# Services Management

## Services Management

### Check Logs

```bash
sudo journalctl -u limonatad -f
```

### Sync Info

```bash
limonatad status 2>&1 | jq .sync_info --home $HOME/.limonata
```

### Node Info

```bash
limonatad status 2>&1 | jq .node_info --home $HOME/.limonata
```

### Your Node Peer

```bash
echo $(limonatad comet show-node-id --home $HOME/.limonata)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.limonata/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Restart Node

```bash
sudo systemctl restart limonatad && sudo journalctl -u limonatad -f
```
