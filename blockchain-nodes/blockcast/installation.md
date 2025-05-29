---
description: BlockCast Installation
---

# Installation

**Windows Users**:

* Must install Ubuntu on Windows using this [guide](https://documentation.codeblocklabs.com/other/wsl), then continue further steps.

**VPS Users**:

* Get your cheap vps from [Contabo](https://lihat.info/contabo), [Vultr](https://lihat.info/vultr) or [VpsAG](https://lihat.info/vpsag)&#x20;

***

### Install Dependecies:

Update packages:

```
sudo apt-get update && sudo apt-get upgrade -y
```

Install Packages:

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

Install Docker:

```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
```

***

### Register Dashboard:

To get started, [register in Dashboard](https://app.blockcast.network/?referral-code=1TEhib)

***

### Install

```
git clone https://github.com/pramonoutomo/blockcast
```

```
cd blockcast
```

***

### Run

```
docker compose up -d
```

* Note: Before procceding to run node, make sure port `18080` is not in-use. If so, open configuration file with `nano docker-compose.yml` and change the port to other, for example to `18081` by replacing `18080:8080` with `18081:8080`.

***

### Check Logs

List Containers:

```
docker compose ps -a
```

Response:

```
NAME                                 IMAGE                             COMMAND                  SERVICE          
beacon-docker-compose-watchtower-1   containrrr/watchtower             "/watchtower"            watchtower
beacond                              blockcast/cdn_gateway_go:stable   "/usr/bin/beacond -l…"   beacond
blockcastd                           blockcast/cdn_gateway_go:stable   "/usr/bin/blockcastd…"   blockcastd
control_proxy                        blockcast/cdn_gateway_go:stable   "/usr/bin/control_pr…"   control_proxy
```

Check logs:

```
docker compose logs -fn 1000
```

* skip logs if you have all containers running.

***

### Register Node

Get location:

```
curl -s https://ipinfo.io | jq '.city, .region, .country, .loc'
```

Generate Node Data & Register:

```
docker compose exec blockcastd blockcastd init
```

* Copy and paste the `Registration URL` from the terminal in browser to open the Dashboard.
* With your Hardware ID and Challenge Key pre-filled, Fill-in your location from previous command.
* Register your Node. Make sure you choose the right location of your machine.
*   Wait a few minutes until your node turn **Online.**

    \


    <figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>
