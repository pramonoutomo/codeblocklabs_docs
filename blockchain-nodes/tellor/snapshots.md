# Snapshots

Please use NodeStake snapshots for tellor as we didn't host any tellor snapshot rn :pray:

```
# Clean data directory
rm -rf $HOME/.layer/data
layerd tendermint unsafe-reset-all --home $HOME/.layer/ --keep-addr-book

# Download snapshot (30+ GB, be patient)
echo "Downloading snapshot..."
SNAP_NAME=$(curl -s https://ss.tellor.nodestake.org/ | egrep -o ">20.*\.tar.lz4" | tr -d ">")
curl -o - -L https://ss.tellor.nodestake.org/${SNAP_NAME} | lz4 -c -d - | tar -x -C $HOME/.layer

echo "Snapshot successfully loaded!"
```

