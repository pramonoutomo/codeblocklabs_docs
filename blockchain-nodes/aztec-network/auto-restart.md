---
description: bash script for auto restart the aztec nodes if there is an error.
---

# Auto Restart

Create new file named run\_aztech.sh

```
sudo nano run_aztec.sh
```

then add code below:

```
#!/bin/bash

# Function to run the aztec command
run_aztec() {
    aztec start --node --archiver --sequencer \
        --network alpha-testnet \
        --l1-rpc-urls http://YOUR_RPC_URL \
        --l1-consensus-host-urls http://YOUR_BEACON_URL \
        --sequencer.validatorPrivateKey YOUR_PRIVATE_KEY\
        --sequencer.coinbase YOUR_ADDRESS \
        --p2p.p2pIp 38.102.84.231 \
        --p2p.maxTxPoolSize 1000000000 \
        --port 8081
}

# Trap Ctrl+C (SIGINT) to exit the script
trap "echo 'Script stopped by user'; exit" SIGINT

# Main loop
while true; do
    echo "Starting aztec command..."
    run_aztec
    
    # If the command fails, wait 5 seconds before restarting
    if [ $? -ne 0 ]; then
        echo "Command failed or exited. Restarting in 5 seconds..."
        sleep 5
    else
        # If the command exited successfully (unlikely in this case), break the loop
        echo "Command exited successfully. Stopping."
        break
    fi
done
```

use CTRL+X , Y , Enter to save the file

allow the file to be executed

```
chmod +x run_aztec.sh
```

to run the script you just need to use command:&#x20;

```
./run_aztec.sh
```

Stopping the command you'll need to use CTRL+C
