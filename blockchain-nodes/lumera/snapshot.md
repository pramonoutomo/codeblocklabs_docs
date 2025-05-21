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
sudo systemctl stop lumerad
cp $HOME/.lumera/data/priv_validator_state.json $HOME/.selfchain/priv_validator_state.json.backup
rm -rf $HOME/.lumera/data
```

```bash
# Download Latest Snapshot
curl -L https://green.codeblocklabs.com/testnet/lumera/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.lumera
mv $HOME/.lumera/priv_validator_state.json.backup $HOME/.lumera/data/priv_validator_state.json
```

```bash
# Restart chain
sudo systemctl start lumerad && sudo journalctl -u lumerad -f -o cat
```
