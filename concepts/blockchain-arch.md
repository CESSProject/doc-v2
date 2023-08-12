The blockchain layer is further divided into five layers:

![Blockchain Architecture](../assets/concepts/blockchain-arch/blockchain-arch.png)

(todo: check to see if the image need to be updated)

**Infrastructure layer** - consists of hardware equipment, including servers, network hardware, and storage hardware for the CESS blockchain.

**Data layer** - supports scalable data storage and provides various data processing algorithms.

**Network layer** - responsible for node connection and data transfer, providing load balancing and P2P network protocols and algorithms.

**Consensus layer** - provides consensus mechanisms on transactions.

**Incentive layer** - achieve fair income distribution through smart contracts and other incentive schemes.

# Infrastructure Layer

CESS invites three types of resources to join the network: server, network, and storage. Server resources focus on computing performance and will perform computing and consensus tasks. Network resources will provide network bandwidth support. Storage resources are the kernel part of the CESS system. With the suitable reward mechanism, we attract nodes with storage capacity to join the network to provide a stable and reliable data storage infrastructure.

# Data Layer

The data layer stores the CESS blockchain and user data files. Encryption algorithms are used in data transmission, storage, and verification to ensure the security and integrity of the data.

# Network Layer

The network layer consists of a P2P storage network and a distributed hash table (DHT) to ensure efficient access to data in the network.

## P2P Storage Network

The network has a distributed application architecture that distributes tasks and workloads among peers. The whole network does not depend on a dedicated centralized server, and each computer in the network acts as both a requestor and responder to requests from other computers to provide resources and services.

## Distributed Hash Table (DHT)

In DHT, each client is responsible for a small range of routing and storing a small portion of the data. They communicate with each other in real-time to enable the addressing and storage of the entire DHT network. As a client connects to any node in the network, it can discover other nodes. DHT technology enables any machine in the network to perform part of the server's functions.

# Consensus Layer

To ensure that the transactions and activities on the blockchain network reach a consensus quickly, CESS proposes a novel Random Rotational Selection(R²S) consensus mechanism based on Byzantine fault tolerance to improve the performance and scalability of the system.

The CESS network has two types of consensus nodes: **on-duty consensus nodes** and **candidate consensus nodes**. Theoretically, any node can become a consensus node via staking. There is no limitation to the number of candidate consensus nodes. CESS uses a credit rating method to verify the nodes and select qualified nodes as consensus candidate nodes. Within each time window, CESS randomly selects the subsequent **eleven consensus nodes** as new on-duty consensus nodes responsible for on-chain transaction validations, data packing, and data block generation. The CESS consensus mechanism aims to achieve network security, system transparency, randomness, and fairness to miners.

# Incentive Layer

CESS's primary system resources are storage and network resources. Miners can provide these two types of resources when joining the CESS network. The network will reward the miners with CESS tokens according to their contributions to the network. CESS has an algorithm to calculate each miner's contribution to the network. The comprehensive algorithm considers the factors of miners' storage capacity, network bandwidth, and node configuration to calculate an overall node score and, thus, the rewards in the form of CESS tokens.

# Distributed Nature of CESS

CESS tokens are distributed in the following ways:

**Storage**: Storage Miners with storage capacity join the network to earn tokens proportional to their bandwidth and storage capacity.

**Consensus** (a.k.a on-chain logics): Consensus Miners with qualified computational resources can stake CESS tokens to run candidate consensus nodes. If selected by CESS random consensus mechanism as one of the on-duty validators, these nodes will earn rewards.

**Community Governance**: Developers, community members, and partners can submit their funding proposals on-chain. These proposals will go through a voting process. When a quorum is established, corresponding proposals will be enacted, and rewards will be issued. Submissions are accepted based on the governance of a Decentralized Autonomous Organization (DAO). CESS community transparently operates the Community Development Fund (CDF) through the voting process.

**Token Distribution**: All incentive-related activities are implemented based on smart contracts, ensuring the fund's distribution fairness and transparency.
