The CESS Network consists of **4** types of nodes: **Consensus Nodes**, **Storage Nodes**, **CDN Nodes**, and **TEE Nodes**. 


{% hint style="success" %}
CESS testnet RPC address: `wss://testnet-rpc.cess.network/ws/`.
{% endhint %}

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
The Storage Node is a fundamental component of the CESS network, acting as the primary entity for data storage and ensuring the integrity of user data. It offers verifiable and efficient idle storage capacity by utilizing the **Proof of Idle Space (PoIS)** mechanism. This mechanism helps identify unused storage resources and enables their repurposing for persistent data storage, all while maintaining high reliability through intelligent space management technology.

To safeguard data, the network employs the **Proof of Data Reduplication and Recovery (PoDR²)** mechanism, which ensures data redundancy and quick recovery in case of data loss. In the event of accidental data loss, the network's storage nodes automatically engage in data recovery procedures to maintain sufficient redundancy, ensuring high data availability.

The CESS chain periodically challenges storage nodes to verify both the availability of idle space and the integrity of the stored data. Storage nodes that successfully pass these challenges are rewarded with incentives for their participation. Users can easily deploy a storage node using simple command-line tools, minimizing configuration efforts while enabling them to monetize idle storage resources.

Storage nodes are responsible for managing their local disk resources, controlling the maximum storage capacity, and ensuring that they deliver data storage, retrieval, and proof calculation services. Nodes that provide larger amounts of storage are eligible for greater rewards, incentivizing the contribution of more substantial storage resources to the network.

{% hint style="success" %}
If you are interested in running a storage node, please refer to the section [**Storage Node**](storage-miner/).
{% endhint %}

# CDN Node
The **CDN Node** refers to nodes within the CESS Content Delivery Network (CDN), which play a vital role in optimizing data retrieval, balancing network load, mitigating DDoS (Distributed Denial of Service) attacks, and facilitating bidirectional data distribution between users and the CESS network. These nodes ensure seamless, low-latency delivery of data, both from users to the network and vice versa. Additionally, CDN Nodes are designed to support the processing of AI-related data, such as training datasets, models, and other AI assets, contributing significantly to the broader AI ecosystem within the CESS framework.

The CDN Node ecosystem is divided into two distinct roles: **Retriever** and **Cacher**.

### **Retriever**
The **Retriever** node is responsible for a range of critical functions, including:
- **Data Retrieval**: Efficiently fetching user data when needed.
- **Caching**: Temporarily storing frequently accessed data to reduce latency.
- **Computation**: Performing necessary processing on data, such as transformations or analytics.
- **Load Scheduling**: Managing data flow and balancing network load.
- **Traffic Proof Verification**: Ensuring that the traffic associated with cached data is valid and effective.

Retrievers share cached data across the CESS network, facilitating decentralized and highly efficient data interoperability within the CDN. Retrievers earn rewards by contributing computational power, data traffic, and supporting the overall performance of the network.

### **Cacher**
The **Cacher** node focuses on **lightweight data caching** and can operate on **low-power DePIN devices**. Its main role is to store and cache frequently requested data, improving network responsiveness. Cachers help scale the edge cache infrastructure by leveraging a large number of low-cost, distributed devices. By contributing to the network's traffic distribution and data availability, cachers earn revenue based on the data they store and serve.

The deployment of **Cachers** enables the **unlimited scalability** of the edge cache, as the CESS network can expand by incorporating numerous small devices that contribute to the overall data distribution and traffic load.

### Monetization and Flexibility
Users can easily deploy either role-**Retriever** or **Cacher**-using simple script tools and dedicated nodes, allowing them to monetize their resources based on the role they choose to run. This flexibility enables a wide range of participants to contribute to the CESS network, whether by providing computational power, storage, or network bandwidth, and earn rewards in return.

# TEE Nodes

The TEE Node operates within the Intel SGX (Software Guard Extensions) trusted execution environment, serving a crucial role in verifying the integrity of idle storage space and managing the initialization of user data, all based on the PoDR² (Proof of Data Reduplication and Recovery) algorithm. By leveraging the secure enclave provided by Intel SGX, the TEE Node ensures that data processing and verification occur in a trusted and tamper-proof environment, adding an extra layer of security and reliability to the network.

The TEE Node functions as an intermediary for consensus nodes, facilitating efficient validation of random challenges related to both idle space availability and the correctness of in-service data. While the TEE Node itself does not directly receive rewards, its operation enhances the overall performance of storage nodes, thereby indirectly benefiting the associated consensus nodes by increasing the likelihood of those nodes being selected as validators.

Users can quickly deploy TEE Nodes on supported devices using straightforward setup scripts. These nodes can be utilized to support the verification of storage nodes or consensus nodes, improving their efficiency and contributing to the scalability and security of the CESS network. This setup enables users to help optimize the consensus process, thus playing a key role in strengthening the overall ecosystem.

The **TEE Node** operates in three distinct modes: **Full Mode**, **Marker Mode**, and **Verifier Mode**, each serving different functions within the CESS network’s trusted execution environment (Intel SGX). These modes offer flexibility depending on the specific role and requirements of the node within the network.

### **Full Mode**
**Full Mode** refers to the comprehensive operational mode of the TEE Node, where it supports all core functions. This includes:
- **Initialization of In-Service Data**: Handling the setup and preparation of active user data.
- **Authentication and Replacement of Idle Space**: Verifying the integrity of unused storage and repurposing it for data storage.
- **Verification of Random Challenges**: Performing random challenge validation for both idle space and in-service data to ensure integrity and availability.

Nodes running in Full Mode perform all the critical tasks needed to verify and maintain data within the CESS network, contributing to the overall security and trustworthiness of the system.

### **Marker Mode**
**Marker Mode** is a more specialized operational mode, where the TEE Node focuses solely on specific tasks excluding random challenge verification. These tasks generally include:
- **Initialization of In-Service Data**: Setting up and managing user data within the trusted environment.
- **Authentication of Idle Space**: Verifying that unused storage space can be safely utilized for data storage.
- **Replacement of Idle Space**: Repurposing idle storage space for active use.

In **Marker Mode**, the TEE Node does not interact with random challenge verification, and does not need to be bound to any specific consensus node. This mode is typically used when only the data authentication and management tasks are required, without the need for challenge-based validation.

### **Verifier Mode**
**Verifier Mode** is focused exclusively on the **random challenge verification** function. Nodes running in this mode validate the challenges related to both idle space and in-service data, ensuring that the data integrity checks are properly executed. To operate in Verifier Mode—or as part of a **Full Mode** configuration that includes this function—the TEE Node must be bound to a **consensus node**. This binding ensures that the node's challenge verification activities are properly coordinated with the consensus mechanism.

Each mode allows the TEE Node to focus on specific tasks, optimizing the resources and operations of the CESS network based on the role it needs to fulfill, whether that is comprehensive data verification, space management, or random challenge validation.

## TEE Worker
The main task of the TEE worker is to mark data (generate file tags) for user's files that are used in PoDR² (Proof of Data Reduplication and Recovery) proofs and to generate space-holder files for the space provided by the storage miners in PoIS (Proof of Idle Space) proofs. Every job completed in the TEE worker is tamper-proof and verifiable, which can effectively ensure data authenticity.

TEE worker is developed based on the [Gramine library](https://gramineproject.io/) and currently only supports [Intel series chips](https://www.intel.com/content/www/us/en/developer/articles/tool/intel-trusted-execution-technology.html).

A TEE worker is bound to a consensus node and can only work after registering a transaction with the account signature of the consensus node. It requires a relatively high hardware requirement and needs the support of TEE functions. To balance out the higher cost, miners also earn a higher reward.

{% hint style="info" %}
If a CESS node is running in both full-node mode and TEE worker mode, it is eligible to be elected as a validator which responsible for producing and verifying blocks.
{% endhint %}

{% hint style="success" %}
If you are interested in running a storage node, please refer to the section [**TEE Node**](tee-node/).
{% endhint %}
