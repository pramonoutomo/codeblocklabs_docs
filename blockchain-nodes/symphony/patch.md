---
description: only for those who getting error different version when upgrading to v5testnet
---

# Patch

Non Cosmovisor

```
cd $HOME 
rm -rf symphony 
git clone https://github.com/Orchestra-Labs/symphony.git
cd symphony
git checkout fix/change-upgrade-name-to-fix-testnet-migration
make install 
sudo systemctl restart symphonyd && sudo journalctl -u symphonyd -f -o cat
```

Cosmovisor

```
cd $HOME 
rm -rf symphony 
git clone https://github.com/Orchestra-Labs/symphony.git
cd symphony
git checkout fix/change-upgrade-name-to-fix-testnet-migration
make build
mv $HOME/symphony/build/symphonyd $HOME/.symphonyd/cosmovisor/upgrades/v5testnet/bin/
```
