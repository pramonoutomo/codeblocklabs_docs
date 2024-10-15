---
description: BlockX Installation
---

# Installation

{% hint style="danger" %}
This testnet installation may already obsolete as the BloxkX already launch it's mainnet.
{% endhint %}

Install Golang

```
// download and install
wget -q -O - https://raw.githubusercontent.com/canha/golang-tools-install-script/master/goinstall.sh | bash -s -- --version 1.18
```

When installation is finished please load variables into system

```
source ~/.profile
```

Check if Go already installed sucessfully

```
// It should return go version go1.18 linux/amd64
go version
```

Run Fullnodes

```
git clone https://github.com/defi-ventures/blockx-node-public-compiled.git
cd blockx-node-public-compiled
./run-fullnode-cosmovisor.sh
```

{% hint style="info" %}
**Useful Tips**\
_Check Version:_ blockxd version --long

_CHECK STATUS BINARY:_ systemctl status blockxd \
_CHECK RUNNING LOGS:_ journalctl -fu blockxd -o cat \
_CHECK LOCAL STATUS:_ curl -s localhost:26657/status | jq .result.sync\_info
{% endhint %}
