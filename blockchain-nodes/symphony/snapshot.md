---
description: Get synced faster and easy.
---

# Snapshot

```bash
sudo apt update
sudo apt-get install snapd lz4 -y
```

```bash
# Stop the service and reset the data
sudo systemctl stop symphonyd
cp $HOME/.symphonyd/data/priv_validator_state.json $HOME/.symphonyd/priv_validator_state.json.backup
rm -rf $HOME/.symphonyd/data
```

```bash
# Download Latest Snapshot
curl -L https://green.codeblocklabs.com/testnet/symphony/snapshot/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.symphonyd
mv $HOME/.symphonyd/priv_validator_state.json.backup $HOME/.symphonyd/data/priv_validator_state.json
```

```bash
# Restart chain
sudo systemctl start symphonyd-testnet && sudo journalctl -u symphonyd -f -o cat
```
