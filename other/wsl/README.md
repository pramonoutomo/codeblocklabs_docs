---
description: Linux on your Windows
---

# WSL

{% hint style="info" %}
Certain nodes task from dev, such as `Contribute Ceremony` or `Contract Deployments`, sometimes don't require a cloud server (VPS). Instead, installing a Linux distribution like Ubuntu on Windows can be sufficient.

We just need to run some command and create a PR (Pull Request) to team github.

In this Guide, I'll tell you how to Install Linux (Ubuntu distribution) on Windows using WSL (Windows Subsystem Linux).
{% endhint %}

## Step 1 - Enable WSL

Open Windows Powershell Terminal.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Run Windows PowerShell as Administrator</p></figcaption></figure>

Run the WSL installation command:

```
wsl --install
```

\*\* It may ask you to choose a username and password.

After the installation complete, please restart your windows machine.

### Step 2: Install Ubuntu

1. Open Microsoft Store:

After restarting, open the Microsoft Store from the Start menu.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Microsoft Store</p></figcaption></figure>

2. Search for Ubuntu:

In the Store, type "Ubuntu" in the search bar. You’ll see various versions like Ubuntu 20.04 LTS, Ubuntu 22.04 LTS, etc

3. Select and Install:

Click on the version you want to install, then click the Get or Install button.

### Step 3: Set Up Ubuntu

1. Launch Ubuntu:

* Once installed, you can launch it directly from the Microsoft Store or by searching for "Ubuntu" in the Start menu
* The first time you launch Ubuntu, it will take a moment to set up. After that, you will be prompted to create a new `user` account and password

## Step 4: Install and Update Packages (aka Drivers)

1. Update and Upgrade default Packages

```
sudo apt update
sudo apt upgrade
```

2. Install more important packages

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

3. Install Docker

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

# Docker version
docker --version
```

_**Optional: To install more packages, please check this.**_

