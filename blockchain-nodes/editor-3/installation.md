# Installation

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

install screen

```
sudo apt update
sudo apt install screen -y
```

UDP Tuning

```
nano /etc/sysctl.conf
```

```
// append the following lines at the end of sysctl.conf
net.core.rmem_max=33554432
net.core.rmem_default=33554432
net.core.wmem_max=33554432
net.core.wmem_default=8388608
net.core.netdev_max_backlog=100000
net.ipv4.udp_rmem_min=8388608

#make the changes effective
sysctl -p
```

### Download The Latest Build <a href="#download-the-latest-build" id="download-the-latest-build"></a>

```
wget https://github.com/raicoincommunity/Raicoin/releases/download/V1.7.0/rai_node
chmod +x rai_node
```

use a new screen for running rai nodes.

```
screen -S rai
```

then choose what running method you want to use.

### Run with Key.dat <a href="#run-with-key.dat" id="run-with-key.dat"></a>

#### 1. Create key pair <a href="#id-1.-create-key-pair" id="id-1.-create-key-pair"></a>

```
./rai_node --key_create --file=key.dat
cd ~/Raicoin
pwd #print where key.dat is stored
```

_**Important===> backup the key.dat and remember the password.**_

#### 2. Create Config <a href="#id-2.-create-config" id="id-2.-create-config"></a>

```
./rai_node --config_create --forward_reward_to=#REPLACE WITH YOUR RAICOIN ACCOUNT CREATED BY https://raiwallet.org#
```

#### 3. Start Node <a href="#id-3.-start-node" id="id-3.-start-node"></a>

```
screen -S node
./rai_node --daemon --key=key.dat
#input password of the key.dat
#press CTRL+a+d to leave screen and let the daemon running
#use 'screen -r node' command to resume screen
```

Record your node account shown when the node starts.

### Run with raw\_key (secret key) <a href="#run-with-raw_key-secret-key" id="run-with-raw_key-secret-key"></a>

#### 1. Create config <a href="#id-1.-create-config" id="id-1.-create-config"></a>

```
./rai_node --config_create --forward_reward_to=rai_1d5ug5e383whh5ajod83m5pdpi9boequwhjurbu4588buwi4hssxdy91guqe
```

#### 2. Run Daemon <a href="#id-2.-run-daemon" id="id-2.-run-daemon"></a>

```
./rai_node --daemon --raw_key
```

then enter your secret key.

### Maximize your revenue \[optional but recommended] <a href="#maximize-your-revenue-optional-but-recommended" id="maximize-your-revenue-optional-but-recommended"></a>

Change your wallet's representative to your node account to get extra reward. When your wallet receives first reward (typically 72 hours later), then click "Settings-->Account Settings", enter the node account(shown in step 3) in "New Representative" field and click "CHANGE REPRESENTATIVE", done!
