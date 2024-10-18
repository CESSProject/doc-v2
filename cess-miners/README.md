CESS Network has two types of nodes, including: **Consensus Nodes** and **Storage Nodes**. 

CESS testnet RPC address: `wss://testnet-rpc.cess.network/ws/`.

# Consensus Nodes

Consensus nodes are essential in validator elections and authoring blocks in the CESS blockchain. All consensus nodes have the following features:

- Record and store the transaction results and state changes
- Communicate among nodes that form a peer-to-peer network in a decentralized fashion
- Execute consensus algorithm to ensure on-chain data's security and sustained growth
- Contain cryptographic algorithm for signatures and transaction verifications

Consensus node is developed using [Substrate](https://substrate.io/) framework.

## CESS Node

CESS Node is a core component in the CESS network. It is a blockchain node program developed based on the Substrate framework.

{% hint style="info" %}
If running TEE worker simultaneously, a CESS node running in full-node is eligible to be elected as a validator, responsible for producing and verifying blocks.
{% endhint %}

## TEE Worker

The main task of the TEE worker is to mark data (generate file tags) for user's files that are used in PoDR2 (Proof of Data Reduplication and Recovery) proofs and to generate space-holder files for the space provided by the storage miners in PoIS (Proof of Idle Space) proofs. Every job completed in the TEE worker is tamper-proof and verifiable, which can effectively ensure data authenticity.

TEE worker is developed based on the [Gramine library](https://gramineproject.io/) and currently only supports [Intel series chips](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-trusted-execution-technology.html).

A TEE worker is bound to a consensus node and can only work after registering a transaction with the account signature of the consensus node. It requires a relatively high hardware requirement and needs the support of TEE functions. To balance out the higher cost, miners also earn a higher reward.

{% hint style="success" %}
If you are interested in running a consensus node, please refer to the section [**Consensus Miners**](consensus-miner/).
{% endhint %}

# Storage Nodes

Storage nodes play a crucial role in the distributed storage system. All nodes are peers and form a globally distributed storage network using P2P communication technology based on [libp2p](https://github.com/libp2p/go-libp2p) implementation.

Storage nodes are responsible for providing storage space, storing data, providing downloads, and calculating data proof. They also control which disk to use and the maximum storage capacity to serve the CESS network. The larger the storage provided, the higher the rewards obtained.

The CESS network incentivizes storage nodes to offer disk capacities of 4GB and above, preferably utilizing SSDs with a network bandwidth of no less than 2Mbps. Storage nodes satisfying this requirement can yield higher rewards at an accelerated pace.

## Chain Client Component

The chain client component is based on the open-source [go-substrate-rpc-client](https://github.com/centrifuge/go-substrate-rpc-client) implementation, which specifies how storage nodes interact with blockchain nodes and provides functions such as viewing chain status, transactions, and listening for events.

## Database Component

The database component adopts a high-performance [LevelDB](https://en.wikipedia.org/wiki/LevelDB) based on the open-source [**goleveldb**](https://github.com/syndtr/goleveldb) implementation. It is used to provide on-chain metadata cache and accelerate reading.

## P2P Network

The P2P communication component is based on [**libp2p**](https://libp2p.io/) development, which realizes the formation of a P2P network between storage nodes. For details, refer to the GitHub repository [CESSProject/p2p-go](https://github.com/CESSProject/p2p-go).

{% hint style="success" %}
If you are interested in running a storage node, please refer to the section [**Storage Node**](storage-miner/).
{% endhint %}
