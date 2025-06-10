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
sudo systemctl stop achillesd 
cp $HOME/.achilles/data/priv_validator_state.json $HOME/.achilles/priv_validator_state.json.backup
rm -rf $HOME/.achilles/data
```

```bash
# Download Latest Snapshot
curl -L https://green.codeblocklabs.com/testnet/odiseo/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.achilles
mv $HOME/.achilles/priv_validator_state.json.backup $HOME/.achilles/data/priv_validator_state.json
```

```bash
# Restart chain
sudo systemctl start achilles && sudo journalctl -u achilles -f -o cat
```
