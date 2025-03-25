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
sudo systemctl stop selfchaind
cp $HOME/.selfchain/data/priv_validator_state.json $HOME/.selfchain/priv_validator_state.json.backup
rm -rf $HOME/.selfchain/data
```

```bash
# Download Latest Snapshot
curl -L https://green.codeblocklabs.com/testnet/selfchain-v2/selfchain-latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.selfchain
mv $HOME/.selfchain/priv_validator_state.json.backup $HOME/.selfchain/data/priv_validator_state.json
```

```bash
# Restart chain
sudo systemctl start selfchaind && sudo journalctl -u selfchaind -f -o cat
```
