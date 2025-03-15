# RPC Command

Check nodes version

```
curl -d '{"action": "version"}' localhost:6900
```

Check nodes version

```
curl -d '{"action": "block_count"}' localhost:6900
```

Create New Nodes Wallet\_ID

```
curl -g -d '{ "action": "wallet_create"}' localhost:6900
```

Import your wallet into nodes

```
curl -g -d '{ "action": "wallet_change_seed", "wallet": "YOUR_NODES_WALLET_ID", "seed": "YOUR_SEED_64char"}' localhost:6900
```

more rpc command are available on official nano pages, since banano are fork from nano, it's RPC are same :) [https://docs.nano.org/commands/rpc-protocol/](https://docs.nano.org/commands/rpc-protocol/)
