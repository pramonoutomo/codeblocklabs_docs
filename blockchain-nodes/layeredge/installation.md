# Installation

**Step 1: Clone the Light Node Repository**

```
git clone https://github.com/Layer-Edge/light-node.git
cd light-node
```

**Step 2: Install Required Dependencies**

Ensure the following dependencies are installed:

* **Go**: Version 1.18 or higher
*   **Rust**: Version 1.81.0 or higher&#x20;

    {% code fullWidth="true" %}
    ```
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```
    {% endcode %}
*   **Risc0 Toolchain**: If not installed, run:

    {% code fullWidth="true" %}
    ```
    curl -L https://risczero.com/install | bash && rzup install
    ```
    {% endcode %}
* **LayerEdge gRPC Endpoint**: Ensure access to a LayerEdge node for seamless communication.

**Step 3: Configure Environment Variables**

Set up the required environment variables in your terminal session or add them to a `.env` file:

```
GRPC_URL=grpc.testnet.layeredge.io:9090
CONTRACT_ADDR=cosmos1ufs3tlq4umljk0qfe8k5ya0x6hpavn897u2cnf9k0en9jr7qarqqt56709
ZK_PROVER_URL=http://127.0.0.1:3001
# Alternatively:
ZK_PROVER_URL=https://layeredge.mintair.xyz/
API_REQUEST_TIMEOUT=100
POINTS_API=https://light-node.layeredge.io
PRIVATE_KEY='cli-node-private-key'
```

Ensure that the `ZK_PROVER_URL` matches the server where the Merkle service is running.

**Step 4: Start the Merkle Service**

Before running the Light Node, start the Merkle service:

```
cd risc0-merkle-service
cargo build && cargo run
```

Wait until the Merkle service is fully initialized before proceeding.

**Step 5: Build and Run the LayerEdge Light Node**

In a separate terminal window, navigate to the root directory and execute:

```
go build
./light-node
```

Ensure that the Light Node is running **independently** and correctly **connected** to the Merkle service.

***

#### **Connecting CLI Node with LayerEdge Dashboard** <a href="#connecting-cli-node-with-layeredge-dashboard" id="connecting-cli-node-with-layeredge-dashboard"></a>

To link your CLI node with the dashboard for **analytics**:

**Fetch Points via CLI**

```
https://light-node.layeredge.io/api/cli-node/points/{walletAddress}
```

Replace `{walletAddress}` with your **actual CLI wallet address**.

**Connect to Dashboard**

1. Navigate to [dashboard.layeredge.io](https://dashboard.layeredge.io/)
2. Connect your wallet
3. Link your CLI node’s Public Key

**Important Notes:**

* One CLI wallet **can only** link to one dashboard wallet.
* Linking is **mandatory**, even if the CLI and dashboard wallets are identical.

**Dashboard Monitoring Features**

* **Node status** (Active/Inactive)
* **Points tracking & detailed analytics**

***

#### **Logging & Monitoring** <a href="#logging-and-monitoring" id="logging-and-monitoring"></a>

The node provides comprehensive logs for:

* **Merkle tree discovery**
* **ZK proof generation and verification**
* **Submission status**
* **Performance optimizations** (tree sleep state)

***

#### **Security Best Practices** <a href="#security-best-practices" id="security-best-practices"></a>

* Always store keys, mnemonics, and AUTHKEY offline.
* Utilize firewall protections and secure SSH configurations.
* Regularly update nodes from official LayerEdge sources.

***

#### **Troubleshooting Common Issues** <a href="#troubleshooting-common-issues" id="troubleshooting-common-issues"></a>

**Issue:** gRPC connection is inactive. **Solution:** Verify gRPC connection settings.

**Issue:** Risc0 prover service is not running. **Solution:** Restart the prover service and check logs.

**Issue:** Incorrect wallet or signature configurations. **Solution:** Double-check wallet addresses and environment variables.

Consult detailed logs for more specific errors.
