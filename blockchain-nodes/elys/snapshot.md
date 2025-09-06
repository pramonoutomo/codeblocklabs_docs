# Snapshot

```
sudo systemctl stop elys

# Backup priv_validator_state.json
cp $HOME/.elys/data/priv_validator_state.json $HOME/.elys/priv_validator_state.json.backup

# Remove old data and wasm
rm -rf $HOME/.elys/data $HOME/.elys/wasm

# Download and extract Highstakes snapshot (.tar.gz)
curl -L https://green.codeblocklabs.com/mainnet/elys/backup_latest.tar.gz | tar -xz -C $HOME/.elys

# Restore priv_validator_state.json
mv $HOME/.elys/priv_validator_state.json.backup $HOME/.elys/data/priv_validator_state.json

# Restart node
sudo systemctl restart elys && sudo journalctl -u elys -f

```
