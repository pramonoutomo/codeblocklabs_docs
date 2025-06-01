---
description: Aztec Network Testnet
---

# Installation

{% hint style="info" %}
You need  to have your own sepolia RPC or rent it from any paid provider, the free one will have limited access and can't be used.
{% endhint %}

### 1. Follow This Step If It's Your First Time Running Aztec Sequencer (Otherwise Just Next To Step 2):

```
sudo apt-get update && sudo apt-get upgrade -y
```

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen  -y
```

```
source <(wget -O - https://raw.githubusercontent.com/frianowzki/installer/main/docker.sh)
```

```
sudo groupadd docker && sudo usermod -aG docker $(whoami) && newgrp docker
```

```
bash -i <(curl -s https://install.aztec.network)
```

```
echo 'export PATH=$PATH:/root/.aztec/bin' >> ~/.bashrc
```

```
source ~/.bashrc
```

```
aztec-up alpha-testnet
```

### 2. Stop, Remove & Update Current Sequencer:

```
docker ps -a
```

```
docker stop [aztec-container-id] && docker rm [aztec-container-id]
```

```
rm -rf .aztec/alpha-testnet/data
```

```
aztec-up alpha-testnet

// OR use below if you need specific version (example: v0.87.2)
aztec-up -v 0.87.2
```

### 12. Now let's run Aztec Sequencer again if there is no error:

```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls CONSENSUS_HOST_URL \
  --sequencer.validatorPrivateKey 0xPrivateKey \
  --sequencer.coinbase 0xPublicAddress \
  --p2p.p2pIp IP \
  --port 8081 
```

* Change `RPC_URL` with:

```
https://Your_Rented_RPC_URL

// OR

http://YourOWN_RPC_IP:PORT
```

* Change `CONSENSUS_HOST_URL` with:

```
https://Your_Rented_Beacon_RPC_URL

// OR

http://YourOWN__BEACON_RPC_IP:PORT
```

* Enter and wait until it fully synced, you can check the logs using:

```
docker ps -a
```

```
docker logs -f [aztec-container-ID]
```

### 13. Update Aztec Governance (Only for Validators):

```
docker ps -a
```

```
docker stop [aztec-container-id] && docker rm [aztec-container-id]
```

```
rm -rf .aztec/alpha-testnet/data
```

```
aztec-up alpha-testnet
```

If you are using CLI you can just add this line

```
--sequencer.governanceProposerPayload 0x54F7fe24E349993b363A5Fa1bccdAe2589D5E5Ef
```

Or if you use Docker Compose you can use this on your .env

```
cd .aztec/alpha-testnet
```

```
nano .env
```

```
ETHEREUM_RPC_URL=http:/your_IP:8545
CONSENSUS_BEACON_URL=http:/your_IP:3500
VALIDATOR_PRIVATE_KEY=0xPrivateKey
COINBASE=0xPublicAddress
P2P_IP=your_IP
GOVERNANCE_PAYLOAD=0x54F7fe24E349993b363A5Fa1bccdAe2589D5E5Ef
```

Save it and set Docker Compose:

```
nano docker-compose.yml
```

```
services:
  aztec-node:
    container_name: aztec-sequencer
    image: aztecprotocol/aztec:alpha-testnet
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEY: ${VALIDATOR_PRIVATE_KEY}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: debug
      GOVERNANCE_PAYLOAD: ${GOVERNANCE_PAYLOAD}
    entrypoint: >
      sh -c "node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start \
      --network alpha-testnet \
      --node \
      --archiver \
      --sequencer \
      --sequencer.governanceProposerPayload ${GOVERNANCE_PAYLOAD}"
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8081:8081
    volumes:
      - /root/.aztec/alpha-testnet/data/:/data
```

Run it by use this command:

```
docker compose up -d
```
