# Services Management

## Services Management

### Check Logs

```bash
sudo journalctl -u latandad -f
```

### Sync Info

```bash
latandad status 2>&1 | jq .sync_info --home $HOME/.latanda
```

### Node Info

```bash
latandad status 2>&1 | jq .node_info --home $HOME/.latanda
```

### Your Node Peer

```bash
echo $(latandad comet show-node-id --home $HOME/.latanda)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.latanda/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Restart Node

```bash
sudo systemctl restart latandad && sudo journalctl -u latandad -f
```
