---
description: Governor Tutorial Only!
---

# Installation

{% hint style="info" %}
You'll need 1,2 INTO tokens to start
{% endhint %}

**Setup the Cli**

```
// install Go
sudo rm -rvf /usr/local/go/
wget https://golang.org/dl/go1.23.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
rm go1.23.4.linux-amd64.tar.gz

// configure Go
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin

// setup the intento cli
git clone https://github.com/trstlabs/intento intento
cd intento
git checkout v1.0.1
make install
```

**Create validator data**

```
// init , change [moniker] to your nodes name
intentod init [moniker] --chain-id intento-1

// grab your key, copy all data shown after command entered.
// example: {"@type":"/cosmos.crypto.ed25519.PubKey","key":"fCUIxxxxxxxxxxxxxxxxx="}
intentod cometbft show-validator

// create validatordata.json
sudo nano validatordata.json
```

\*enter script below for validatordata.json

```
// validatordata.json
{
    "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"YOURKEY_HERE="},
    "amount": "1200000uinto",
    "moniker": "YOUR_GOVERNOR_NAME",
    "identity": "YOUR_KEYBASE",
    "website": "YOUR_WEBSITE",
    "security": "YOUR_EMAIL",
    "details": "YOUR_NODES_DETAILS",
    "commission-rate": "0.1",
    "commission-max-rate": "0.1",
    "commission-max-change-rate": "0.05",
    "min-self-delegation": "1"
}
```

CTRL+X , Y , Enter to save.

**Create / Import Your Wallet and fill it with some INTO tokens.**

```
// Add Wallet
intentod keys add wallet

// OR //

// Import Wallet
intentod keys add wallet --recover
// then insert your phrase
```

**Deploy Governor**

```
// Deploy
intentod tx staking create-validator validatordata.json --from wallet --chain-id intento-1 --gas auto --gas-adjustment 2 --node https://rpc.intento.ccnodes.com/ --fees auto --gas auto
```

{% hint style="info" %}
In this case i'm using ccnodes rpc, you can use any public intento rpc. ofcourse you can also use your own intento rpc. \
Running Intento RPC nodes aren't required for governor, but you may/may not receive delegation from team if not running complete services.
{% endhint %}
