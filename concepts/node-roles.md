The CESS network mainly has two node types: **Consensus Nodes** and **Storage Nodes**.

# Consensus Nodes

Consensus nodes are essential in validator elections and authoring blocks in the CESS blockchain. All consensus nodes have the following features:

- Record and store the transaction results and state changes
- Communicate among nodes that form a peer-to-peer network in a decentralized fashion
- Execute consensus algorithm to ensure on-chain data's security and sustained growth
- Contain cryptographic algorithm for signature and transaction verifications

Consensus node is developed using [Substrate framework](https://substrate.io/).

## CESS Node

CESS Node is a core component in the CESS network. It is a blockchain node program developed based on the Substrate framework.

{% hint style="info" %}
If running TEE Worker simultaneously, a CESS node running in the full node mode will be elected as a validator, responsible for producing and verifying blocks.
{% endhint %}

## TEE Worker

The main task of the TEE Worker is to mark data (generate file tags) for user's files that are used for PoDR2 (Proof of Data Reduplication and Recovery) proofs and to generate space-holder files for the space provided by the storage miners for PoIS (Proof of Idle Space) proofs. Every job completed in [TEE](https://en.wikipedia.org/wiki/Trusted_execution_environment) is tamper-proof and verifiable, which can effectively ensure the authenticity of data.

TEE Worker is developed based on the [Gramine library](https://gramineproject.io/) and currently only supports [Intel series chips](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-trusted-execution-technology.html).

A TEE Worker is bound to a consensus node and can only work after registering transactions with the account signature of the consensus node. It requires high hardware performance, needs hardware support of TEE functions, and entails a powerful CPU to complete the marking task quickly. Thus this has a higher cost but also earns more rewards for a miner.

If you are interested in running a consensus node, please refer to the section [**Role: Consensus Miners**](../consensus-node).

# Storage Nodes

Storage nodes play another crucial role in the distributed storage system of the CESS network. All nodes are peers and form a globally distributed storage network using P2P communication technology based on [libp2p](https://github.com/libp2p/go-libp2p) implementation.

Storage nodes are responsible for providing storage space, storing data, providing downloads, and calculating data proof. They also control which disk to use and the maximum storage capacity to serve the CESS network. The larger the storage provided, the higher the rewards obtained.

The CESS network incentivizes storage nodes to offer disk capacities of 4TB and above, preferably utilizing SSDs, with a network bandwidth of no less than 2Mbps. Storage nodes satisfying this requirement can yield higher rewards at an accelerated pace.

## Chain Client Component

The chain client component is based on the open-source [go-substrate-rpc-client](https://github.com/centrifuge/go-substrate-rpc-client) implementation, which specifies how storage nodes interact with blockchain nodes and provides functions such as viewing chain status, transactions, and listening for events.

## Database Component

The database component adopts a high-performance [LevelDB](https://en.wikipedia.org/wiki/LevelDB) based on the open-source [**goleveldb**](https://github.com/syndtr/goleveldb) implementation. It is used to provide on-chain metadata cache and accelerate reading.

## P2P communication component

The P2P communication component is based on [**libp2p**](https://libp2p.io/) development, which realizes the formation of a P2P network between storage nodes. For details, refer to the GitHub repository [CESSProject/p2p-go](https://github.com/CESSProject/p2p-go).

If you are interested in running a storage node, please refer to the section [**Roles: Storage Miners**](../storage-node).
