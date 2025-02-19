The CESS Network consists of **4** types of nodes: **Consensus Nodes**, **Storage Nodes**, **TEE Nodes**, and **CDN Nodes**. 

CESS testnet RPC address: `wss://testnet-rpc.cess.network/ws/`.

# Consensus Nodes

The Consensus Node is the foundational builder of the CESS blockchain, responsible for packaging and publishing blocks through the proprietary **R²S** consensus mechanism. R²S extends the classic PoS algorithm by introducing a dynamic selection process, choosing **11** consensus nodes as validators in each cycle. This selection is based on node workload, the number of staked tokens, and a randomized factor. Validators play a key role in block production and confirmation, earning CESS tokens as rewards for their efforts. 
All consensus nodes have the following features:

- Record and store the transaction results and state changes
- Communicate among nodes that form a peer-to-peer network in a decentralized fashion
- Execute consensus algorithm to ensure on-chain data's security and sustained growth
- Contain cryptographic algorithm for signatures and transaction verifications

Users can either run their own consensus node to become a **Validator**, or stake tokens and be a **Nominator** to support  validators, participating in the block production rewards.

## Validator
The Validator is consensus node selected by the consensus algorithm within a specific period. They are responsible for block packaging and confirming the consistency of blockchain state within the current era. Validators who successfully produce blocks will receive block rewards.

## Nominator
The Nominator is a cryptocurrency holder who indirectly participates in blockchain consensus and shares profits by supporting validators through collateral tokens.

{% hint style="success" %}
If you are interested in running a consensus node, please refer to the section [**Consensus Nodes**](consensus-miner/).
{% endhint %}

# Storage Nodes
Storage Node is the main carrier for storing user data in the CESS network. It provides verifiable and effective idle storage space for the CESS network through the **Proof of Idle Space (PoIS)** mechanism, replaces sufficient idle space with intelligent space management technology to persistently store user data, and ensures the reliability of user data through the **Proof of Data Reduplication and Recovery (PoDR²)**. When some user data is accidentally lost, the entire network storage node will automatically perform data recovery to ensure that there is always enough redundancy to ensure data availability. The CESS network will randomly challenge storage nodes from time to time to verify the validity of their idle space and stored user data, and issue challenge rewards to storage nodes that pass the verification. Users can use node running script tools to quickly build a storage node with a small amount of configuration and commands, and join the CESS network to achieve resource monetization.

Storage nodes play a crucial role in the distributed storage system. All nodes are peers and form a globally distributed storage network using P2P communication technology based on [libp2p](https://github.com/libp2p/go-libp2p) implementation.

Storage nodes are responsible for providing storage space, storing data, providing downloads, and calculating data proof. They also control which disk to use and the maximum storage capacity to serve the CESS network. The larger the storage provided, the higher the rewards obtained.

The CESS network incentivizes storage nodes to offer disk capacities of 4GB and above, preferably utilizing SSDs with a network bandwidth of no less than 2Mbps. Storage nodes satisfying this requirement can yield higher rewards at an accelerated pace.

{% hint style="success" %}
If you are interested in running a storage node, please refer to the section [**Storage Node**](storage-miner/).
{% endhint %}

# TEE Nodes

TEE Node is a node running in the Intel SGX trusted execution environment. It mainly authenticates the legality of idle space of storage nodes and initializes stored user data based on the PoDR² algorithm through the trusted execution environment. It also acts as a proxy for consensus nodes to achieve efficient verification of random challenge proofs of idle space and inservice data. Running TEE Node does not directly gain any benefits, but it can improve the efficiency of storage nodes and accumulate workload for their proxy consensus nodes to help them increase the probability of successfully becoming validators. Users can use node running script tools to quickly set up TEE Nodes on devices that meet the requirements, serving their own storage nodes or consensus nodes.
TEE Node has three working modes: **Full Mode**, **Marker Mode** and **Verifier Mode**.

## Full Mode
The full mode of TEE Node refers to a running mode that supports all functions of TEE Node, which usually includes functions such as initialization of inservice data, authentication and replacement of idle space, and verification of random challenges of idle space and inservice data.
Marker Mode

## Marker Mode
The marker mode of TEE Node refers to a running mode of TEE Node that only supports functions other than random challenge verification. It usually only includes initialization of inservice data, authentication of idle space, and replacement functions. TEE Node running in this mode does not need to be bound to a specific consensus node.

## Verifier Mode
The verifier mode of TEE Node refers to a running mode of TEE Node that only supports random challenge verification function. Running this mode or a full mode that includes this mode function requires binding a consensus node to the TEE Node.

## TEE Worker
The main task of the TEE worker is to mark data (generate file tags) for user's files that are used in PoDR² (Proof of Data Reduplication and Recovery) proofs and to generate space-holder files for the space provided by the storage miners in PoIS (Proof of Idle Space) proofs. Every job completed in the TEE worker is tamper-proof and verifiable, which can effectively ensure data authenticity.

TEE worker is developed based on the [Gramine library](https://gramineproject.io/) and currently only supports [Intel series chips](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-trusted-execution-technology.html).

A TEE worker is bound to a consensus node and can only work after registering a transaction with the account signature of the consensus node. It requires a relatively high hardware requirement and needs the support of TEE functions. To balance out the higher cost, miners also earn a higher reward.

{% hint style="info" %}
If a CESS node is running in both full-node mode and TEE worker mode, it is eligible to be elected as a validator which responsible for producing and verifying blocks.
{% endhint %}

# CDN Node
CDN Node is a collective term for nodes in the CESS CDN network, responsible for implementing data retrieval load balancing, preventing DDOS attacks, and bidirectional distribution of user data from users to the CESS network and from the CESS network to users. Moreover, CDN Node also implements native support for processing AI data/training datasets, models and other data in the underlying architecture, helping CESS contribute to the development of the AI ecosystem. CDN Node can be divided into two roles in the program: retriever and cacher. Cachers can run on a large number of DePin devices to achieve lightweight data caching functions and earn revenue by contributing data traffic; retrievers support functions such as data retrieval and processing, load scheduling, and data sharing between nodes, and earn revenue by contributing computing power and data traffic. Users can run script tools through dedicated nodes to monetize related resources by running any role of CDN Node as needed.
CDN Node includes **Retriever** and **Cacher**.

## Retriever
The retriever includes data retrieval, caching, calculation, and scheduling functions, and verifies effective traffic proof for the cache. The retriever shares cached data through the IPFS network, achieving efficient data interoperability across the entire CDN network.
## Cacher
The cacher realizes lightweight data caching function and can run on low-power DePin devices, contributing traffic to obtain revenue. CESS achieves unlimited scalability of edge cache by deploying a large number of cachers.

{% hint style="success" %}
If you are interested in running a storage node, please refer to the section [**TEE Node**](tee-node/).
{% endhint %}

