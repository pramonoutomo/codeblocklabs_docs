---
icon: power-off
---

# What is Node?

A blockchain node is a device or computer that participates in a blockchain network by maintaining a copy of the blockchain ledger and processing transactions. These nodes perform essential functions that support the decentralized and secure nature of the blockchain.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Blockchain</p></figcaption></figure>

Here’s a breakdown of the key roles and types of blockchain nodes:

#### 1. **Full Nodes**

* **Definition**: These nodes store the entire history of the blockchain. They validate transactions and blocks by checking them against the blockchain's rules.
* **Function**: They ensure the network's integrity by rejecting invalid transactions and blocks. They also broadcast verified data to other nodes.
* **Example**: Bitcoin and Ethereum full nodes.

#### 2. **Lightweight Nodes (Light Clients)**

* **Definition**: These nodes do not store the full blockchain but rely on full nodes to verify transactions. They store only essential data (like block headers) and request specific information when needed.
* **Function**: They allow users to interact with the blockchain without the need for large storage or extensive processing power.
* **Example**: Wallet apps like MetaMask for Ethereum.

#### 3. **Mining Nodes**

* **Definition**: These nodes are full nodes with the added responsibility of mining (proof of work) or validating blocks (proof of stake).
* **Function**: They participate in the process of creating new blocks by solving complex mathematical problems (in proof of work) or validating based on staked assets (in proof of stake).

#### 4. **Validator Nodes**

* **Definition**: Used primarily in proof-of-stake (PoS) blockchains, validator nodes verify and validate new transactions and propose new blocks.
* **Function**: They maintain the security and operation of PoS blockchains by staking tokens and voting on block proposals.

#### 5. **Archive Nodes**

* **Definition**: Similar to full nodes but store additional historical data beyond the latest state of the blockchain.
* **Function**: These nodes provide detailed historical data for developers, researchers, or services that need access to the complete history of transactions and contracts.

#### Key Functions of Blockchain Nodes:

* **Transaction Verification**: Nodes verify that transactions follow the rules of the blockchain (e.g., ensuring the sender has enough funds).
* **Block Validation**: Full nodes check each new block to confirm it adheres to consensus rules before adding it to their copy of the blockchain.
* **Propagation of Data**: Nodes share data (blocks, transactions) with other nodes to maintain network synchronization.
* **Network Security**: Nodes contribute to the decentralized nature of the network, making it harder for a single entity to control the blockchain.

Nodes are essential for maintaining decentralization and transparency in blockchain systems, ensuring that no single party can control the entire ledger or manipulate transactions.

{% hint style="info" %}
Every blockchain has a different nodes type, some has only mining/validator nodes, some has Full and Light nodes only and many others.
{% endhint %}
