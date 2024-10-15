# Installation

### Installation (Docker) <a href="#installation-docker" id="installation-docker"></a>

Install docker from [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

then use command below to install and run banano nodes automatically on port 6900.

```
docker run --restart=unless-stopped -d -p 6900:6900-v ~:/root bananocoin/banano
```

or if you want to use different folder or different port:

```
docker run --restart=unless-stopped -d \
  -p 7071:7071 \
  -p [::0]:7072:7072 \
  -p [::0]:7074:7074 \
  -v /mnt/blockstorage:/root \
  bananocoin/banano
```

just change the data on -v and port on -p (it's also specified more port for accessing nodes from outside)

To check logs, use command:

```
docker log
```

### Installation (sourcecode) <a href="#installation-sourcecode" id="installation-sourcecode"></a>

#### install dependencies <a href="#install-dependencies" id="install-dependencies"></a>

```
sudo apt update;
sudo apt upgrade -y;
sudo apt-get install -y git cmake make g++ curl wget python-dev;
sudo apt install python3-pip;
sudo pip install cmake-format;
```

#### create swap (only if your nodes are not having a good configuration) <a href="#create-swap-only-if-your-nodes-are-not-having-a-good-configuration" id="create-swap-only-if-your-nodes-are-not-having-a-good-configuration"></a>

```
sudo fallocate -l 4G /swapfile;
sudo chmod 600 /swapfile;
sudo mkswap /swapfile;
sudo swapon /swapfile;
sudo nano /etc/fstab;

# add the below line to /etc/fstab
/swapfile swap swap defaults 0 0
```

**Installing precompiled Boost**

```
sudo apt install -y libboost-all-dev
```

**Building static Boost**

_**automatic build**_

```
sudo banano_build/util/build_prep/bootstrap_boost.sh --m
```

\*\*the --m flags are optional, it may reduce time, but may break Boost for other use cases than building the bananode

_**manual build**_

```
wget https://netix.dl.sourceforge.net/project/boost/boost/1.70.0/boost_1_70_0.tar.gz   
tar xzvf boost_1_70_0.tar.gz   
cd boost_1_70_0   
./bootstrap.sh --with-libraries=system,thread,log,filesystem,program_options,coroutine,context  
./b2 --prefix=/usr/local/boost link=static install   
cd ..
```

**Cloning the Bananode's git repository**

**check out master branch**

```
git clone --recursive https://github.com/BananoCoin/banano.git banano_build   
```

**check out V23.3 branch.**

```
git clone --branch v23.3 --recursive https://github.com/BananoCoin/banano.git banano_build
```

NOTE: rocksdb needs to be updated or you get a compiler error regarding `block_rep` The below code updates rocksdb to the v24 version of rocksdb:

```
cd banano_build
cd rocksdb
git checkout 79f08d7ffa6d34d9ca3357777bcb335884a56cfb
cd ..
cd ..
```

**check out V24 branch.**

```
git clone --branch v24 --recursive https://github.com/BananoCoin/banano.git banano_build
```

**Building bananode**

```
cd banano_build   

git submodule update --init --force --recursive

cmake -G "Unix Makefiles"   
make bananode
```

**Configuring bananode**

```
## copy the node executable into the parent directory
cp bananode ../bananode;
cd ..;
## check if the freshly compiled executable executes 
./bananode --diagnostics;
## generate default config files with all available options. You can then customize the config files, for example to activate RPC or activate voting.
mkdir BananoData;
./bananode --generate_config node > /root/BananoData/config-node.toml 
./bananode --generate_config rpc > /root/BananoData/config-rpc.toml
cp ./banano_build/docker/node/config/config-* BananoData;
## you can delete the banano_build directory now, if you want
```

**Running the bananode**

```
# start the node in the background
screen -dmSL bananode ./bananode --daemon;

# check that it has peers (firewall may prevent peers)
curl -g -d '{"action": "peers"}' 'localhost:7072';

# check local block count (RPC)
curl -g -d '{"action": "block_count"}' 'localhost:7072';

# check local block count (CLI)
./bananode --debug_block_count;

# check kalium block count (RPC)
curl -g -d '{"action": "block_count"}' 'https://kaliumapi.appditto.com/api';

# check how many accounts are still being processed.
cat BananoData/log/* | grep -e 'accounts in pull queue' | tail -n 2
```
