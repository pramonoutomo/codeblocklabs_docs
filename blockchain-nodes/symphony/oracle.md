# Oracle

#### Install Oracle <a href="#install-oracle" id="install-oracle"></a>

```plaintext
cd $HOME
rm -rf symphony-oracle-voter
git clone https://github.com/cmancrypto/symphony-oracle-voter.git
cd symphony-oracle-voter
git checkout v0.0.4r3
```

#### If Update from v0.0.4r2, please use code below <a href="#if-update-from-v004r2" id="if-update-from-v004r2"></a>

```plaintext
cd $HOME/symphony-oracle-voter
git pull 
git checkout v0.0.4r3
```

***

#### Make File <a href="#make-file" id="make-file"></a>

```plaintext
nano $HOME/symphony-oracle-voter/.env
```

#### Edit with Ur Address and Valoper and Password <a href="#edit-with-ur-address-and-valoper-and-password" id="edit-with-ur-address-and-valoper-and-password"></a>

```plaintext
VALIDATOR_ADDRESS=symphonyvaloper************
VALIDATOR_ACC_ADDRESS=symphony***************
KEY_PASSWORD=********
SYMPHONY_LCD = https://symphony-apitest.codeblocklabs.com/
TENDERMINT_RPC= https://symphony-rpctest.codeblocklabs.com/
```

#### Install Python <a href="#install-python" id="install-python"></a>

```plaintext
sudo apt install python3.12
sudo apt install python3.12-venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
deactivate
```

#### Make System Service <a href="#make-system-service" id="make-system-service"></a>

```plaintext
sudo tee /etc/systemd/system/symphony-oracle.service > /dev/null << EOF
[Unit]
Description=Symphony Oracle
After=network.target

[Service]
# Environment variables
Environment="SYMPHONYD_PATH=/root/go/bin/symphonyd"
Environment="PYTHON_ENV=production"
Environment="LOG_LEVEL=INFO"
Environment="DEBUG=false"

# Service configuration
Type=simple
User=root
WorkingDirectory=/root/symphony-oracle-voter
ExecStart=/root/symphony-oracle-voter/venv/bin/python3 -u /root/symphony-oracle-voter/main.py
Restart=always
RestartSec=3
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF
```

#### Starting Services <a href="#start" id="start"></a>

```plaintext
sudo systemctl daemon-reload
sudo systemctl enable symphony-oracle.service
sudo systemctl start symphony-oracle.service
journalctl -u symphony-oracle.service -f
```
