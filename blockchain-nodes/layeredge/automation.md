---
description: To keep the provers restart when it's died.
---

# Automation

Create new file auto.sh

```
sudo nano auto.sh
```

Paste Code below to the file and save the file with CTRL+X , Y and Enter

```
#!/bin/bash

# Path to the light-node executable
LIGHT_NODE="./light-node"

# Infinite loop to keep the script running
while true; do
    # Run the light-node
    $LIGHT_NODE

    # Check the exit status of the light-node
    if [ $? -eq 0 ]; then
        echo "light-node exited normally. Restarting in 5 seconds..."
    else
        echo "light-node crashed or exited with an error. Restarting in 5 seconds..."
    fi

    # Wait for 5 seconds before restarting
    sleep 5
done
```

Give permission to the file auto.sh

```
chmod +x auto.sh
```

Run the automation

```
./run-light-node.sh
```

