# Services Management

## Services Management

### Check Logs

```bash
sudo journalctl -u qied -f
```

### Sync Info

```bash
qied status 2>&1 | jq .sync_info --home $HOME/.qie
```

### Node Info

```bash
qied status 2>&1 | jq .node_info --home $HOME/.qie
```

### Your Node Peer

```bash
echo $(qied comet show-node-id --home $HOME/.qie)'@'$(wget -qO- eth0.me)':'$(cat $HOME/.qie/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Restart Node

```bash
sudo systemctl restart qied && sudo journalctl -u qied -f
```
