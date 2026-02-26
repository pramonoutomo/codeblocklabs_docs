# Installation

Update and Install Dependencies

```
apt update -y && apt upgrade -y && apt autoremove -y && apt install screen zip unzip curl -y
```

Set up Docker's `apt` repository

```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

Install the Docker packages.

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

{% hint style="info" %}
Note

The Docker service starts automatically after installation. To verify that Docker is running, use:

```console
 sudo systemctl status docker
```

Some systems may have this behavior disabled and will require a manual start:

```console
 sudo systemctl start docker
```
{% endhint %}

Download & Extract From Snapshots

```
wget http://45.76.1.67/raiblocksone_202602.zip
unzip -o raiblocksone_202602.zip
rm -rf raiblocksone_202602.zip
rm -rf __MACOSX
```

Run Nodes

```
// To Run With Root Account use command below:
docker run -d --name xro --restart unless-stopped -p 0.0.0.0:8075:8075 -p 127.0.0.1:8076:8076 -p 127.0.0.1:8078:8078 -e prefix="xro_" -e name="RaiblocksOne" -e account="xro_3oh6tp9b8y65w1fzp3aaxgebptkdhnrziiqaairqfhzkcntiwn1r97cgzikp" -e source="D5E4D58E937883E01BFB0508EB989B6A4B7D31F842E8443176BFF255350E5018" -e work="9da7e03b2a2ec54b" -e signature="FBB7588466EAA76196181C3CDF249178BAC70929E28AABBD5DE284362A34899AB4C972A232824067F439E89F2672B792105D3A55763B852FD21967EC7957260D" -e peering="peering.raione.cc" -e peering_port=8075 -e RPC_PORT=8076 -e WS_PORT=8078 -v /root/raiblocksone:/root yxse/nan

// Or Run With Another User, for example username is "ubuntu"
docker run -d --name xro --restart unless-stopped -p 0.0.0.0:8075:8075 -p 127.0.0.1:8076:8076 -p 127.0.0.1:8078:8078 -e prefix="xro_" -e name="RaiblocksOne" -e account="xro_3oh6tp9b8y65w1fzp3aaxgebptkdhnrziiqaairqfhzkcntiwn1r97cgzikp" -e source="D5E4D58E937883E01BFB0508EB989B6A4B7D31F842E8443176BFF255350E5018" -e work="9da7e03b2a2ec54b" -e signature="FBB7588466EAA76196181C3CDF249178BAC70929E28AABBD5DE284362A34899AB4C972A232824067F439E89F2672B792105D3A55763B852FD21967EC7957260D" -e peering="peering.raione.cc" -e peering_port=8075 -e RPC_PORT=8076 -e WS_PORT=8078 -v /home/ubuntu/raiblocksone:/root yxse/nan
```

***

Useful RPC Command

```
// check blocks
curl -g -d '{ "action": "block_count"}' 'localhost:8076'

// check version
curl -g -d '{ "action": "version"}' 'localhost:8076'

// Create New Wallet ID
curl -g -d '{ "action": "wallet_create"}' localhost:8076

// Create New Wallet Address Inside Wallet ID
curl -g -d '{ "action": "account_create","wallet": "WALLET_ID"}' localhost:8076

// Import Your Old Wallet
curl -g -d '{ "action": "wallet_change_seed", "wallet": "WALLET_ID", "seed": "YOUR_OWN_SEED"}' localhost:8076

```

Useful CLI Command

```
// Backup Wallet (Getting your SEED & Private Key for each address)
docker exec -it xro nan_node --wallet_decrypt_unsafe --wallet=WALLET_ID

// Backup Wallet (If you set password before)
docker exec -it xro nan_node --wallet_decrypt_unsafe --wallet=WALLET_ID --password=YOUR_PASSWORD

```

{% hint style="info" %}
**Note**

Please get in touch with us if the snapshots not available for download. we'll try our best to mirror it to another host.
{% endhint %}

<br>
