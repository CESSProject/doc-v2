# ✨ Technical Highlight

## 1. Data Recovery Guarantee

The **Proof of Data Reduplication and Recovery (PoDR²)** protocol is designed to ensure the validity and availability of stored data by continuously challenging storage nodes. To prevent data loss and maintain data integrity in any situation, such as when storage nodes are offline or leaving the network, data fragments are stored redundantly.

## 2. Smart Space Verification (on Storage Node)

**Continuous Availability Proof of Storage (CAPoS)** is implemented to utilize all available storage space scattered throughout the network. This cryptographic algorithm regularly verifies the authenticity and availability of data through interactively audit responses without retrieving the data. Our storage node client also performs regular check on disk status and storage space, cleaning up invalid data to maximize storage resources.

## 3. Unified Storage Price

Unlike the storage space bidding of contemporary decentralized storage protocol, users of CESS only need to bid on a single price point on CESS storage. The CESS middle layer has a pricing mechanism to calculate an optimized price point for storage users while balancing the incentives of storage providers.

## 4. IPFS compatiblity

{% hint style="info" %}
Launch in 2023 product roadmap
{% endhint %}

CESS is compatible with IPFS standard and allows developers to integrate their dApps with an array of storage solutions that utilize IPFS. Data stored on CESS can also be addressed with cryptographic hashes, making the content immutable and tamper-resistant.

## 5. Secure Data Access

{% hint style="info" %}
Launch in 2023 product roadmap
{% endhint %}

**Proxy Re-encryption Technology (PReT)** is built on top of Public Key Encryption to allow users to authorize decryption permissions to others without disclosing the data's contents. User data is also processed and accessed in a Trusted Execution Environment (TEE) within the storage nodes.

## 6. Performance Fast Data Retrieval

{% hint style="info" %}
Launch in 2023 product roadmap
{% endhint %}

Data indexing and Decentralized Content Delivery Network support (DCDN) are used to improve searching and download speed from user endpoints. Consensus mechanism has been iterated on top of [Polkadot GRANDPA](https://wiki.polkadot.network/docs/learn-consensus#finality-gadget-grandpa) to achieve lower gas fees and higher transaction processing speeds.

## 7. Data Ownership Traceability

{% hint style="info" %}
Launch in future
{% endhint %}

Data ownership can be verified with **Multi-format Data Rights Confirmation technology (MDRC)**. This protocol extracts fingerprint from all data and permanently stores them on-chain, in smart contracts, for traceability.
