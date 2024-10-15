# Node Configuration

Configuration for config-node.toml

```
[node]
# throttle bandwidth (recommended unless on a large vps)
# bandwidth_limit = 1

# only allow one person to bootstrap from you at a time (recommended unless on a large vps or a principle rep)
# bootstrap_connections_max = 1

# don't allow burst mode (recommended unless on a large vps or a principle rep)
# bandwidth_limit_burst_ratio = 1.0

[node.rocksdb]
# only enable if you are using rocksdb, by default it's false
enable = true

[node.websocket]
# WebSocket server bind address.
# type:string,ip
address = "::ffff:0.0.0.0"

# Enable or disable WebSocket server. disable unless you need it.
# type:bool
enable = false

[rpc]
# Enable or disable RPC. 
# enable to allow debugging of bootstrapping. disable if not needed after bootstrapping.
# type:bool
enable = true
```

Configuration for config-rpc.toml

```

# Bind address for the RPC server.
# type:string,ip
address = "::ffff:0.0.0.0"

# Enable or disable control-level requests.
# WARNING: Enabling this gives anyone with RPC access the ability to stop the node and access wallet funds.
# type:bool
enable_control = true

# Maximum number of levels in JSON requests.
# type:uint8
max_json_depth = 20

# Maximum number of bytes allowed in request bodies.
# type:uint64
max_request_size = 33554432

# Listening port for the RPC server.
# type:uint16
port = 6900

[logging]

# Whether to log RPC calls.
# type:bool
#log_rpc = true

[process]

# Number of threads used to serve IO.
# type:uint32
#io_threads = 4

# Address of IPC server.
# type:string,ip
#ipc_address = "::1"

# Listening port of IPC server.
# type:uint16
#ipc_port = 46000

# Number of IPC connections to establish.
# type:uint32
#num_ipc_connections = 1
```
