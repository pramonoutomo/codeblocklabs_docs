# Installation

Update and Install Dependencies

```
apt update -y && apt upgrade -y && apt autoremove -y && apt install screen curl -y
```

Install Docker

```
apt install docker.io -y
```

Pull Images From Docker Hub

```
docker pull raiblocksone/raione:R1_V.02
```

Run Nodes Once To Get Settings File

```
docker run --restart=unless-stopped -d -p 7075:7075 -p 127.0.0.1:7076:7076 -p 127.0.0.1:7078:7078 -v /root/raiblocksone/:/root --name raione-node raiblocksone/raione:R1_V.02 
```

Stop Nodes

```
docker stop raione-node
```

Enable Voting

```
sudo nano raiblocksone/Nano/config-node.toml
```

copy this code below to the config-node.toml

```
// Copy this code at the top of file config-node.toml
[node]

enable_voting = true
```

Change RPC Settings

```
// change the value of enable_control to true
sudo nano raiblocksone/Nano/config-rpc.toml
```

Restart The Services

```
docker restart raione-node
```

{% hint style="info" %}
**Useful Command**\
\
_Check Version:_ curl -g -d '{ "action": "version"}' 'localhost:7076'

_Block Count_: curl -g -d '{ "action": "block\_count"}' 'localhost:7076'

_Create New Wallet ID:_ curl -g -d '{ "action": "wallet\_create"}' 'localhost:7076' _Create New Wallet Account:_ curl -g -d '{ "action": "account\_create", "wallet": "%WALLET\_ID% "}' 'localhost:7076'
{% endhint %}

\
