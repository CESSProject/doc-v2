The CESS Network has strategically chosen to allocate a significant portion of its Total Token Supply (TTS) to mining rewards—an initiative aimed at strengthening its ecosystem. This allocation clearly reflects the network's recognition of the value contributed by its participants. The rationale behind this distribution is as follows:

**Consensus Node Operators**: 
As the backbone of the CESS Network, consensus node operators provide essential computing power, bandwidth, and storage. The incentive fund is designed to attract more operators and ensure their continued commitment.

**Storage Node Providers**: 
The network relies on storage nodes to supply storage resources, along with some bandwidth and computing power. By appropriately incentivizing them, CESS attracts the most efficient and reliable resource providers.

**CD²N Node Providers (Content Decentralized Delivery Network)**: 
A large number of decentralized CD²N nodes contribute bandwidth and caching resources, forming high-speed, cross-regional data delivery bridges between the CESS storage network and application endpoints. Incentivizing these nodes is key to building a more robust and efficient content delivery network.

In short, mining rewards are more than just a form of distribution—they are an investment. CESS is making a long-term commitment to its community, ensuring that the platform is not only viable today but remains strong and sustainable well into the future.

# Storage nodes
## Mining Rewards
Storage Nodes must pass random on-chain challenges in order to earn mining rewards. The amount of the reward depends on the reward pool for that challenge round and the proportion of the miner's storage power relative to the total network. Once rewards are issued, Storage Nodes must manually initiate a transaction to claim them.

The CESS Network uses a challenge cycle of every 4 Eras (approximately 24 hours) to take snapshots of miners who have passed challenges, and rewards are calculated based on these snapshots. The reward pool for a given challenge cycle is sourced from a fixed number of tokens distributed over those 4 Eras. (Currently, each Era lasts about 6 hours, and around 324,688 $CESS are issued per Era. This follows a halving every 4 years rule, with a portion of each issuance allocated to the reward pool.)
During the k-th challenge of the N-th challenge cycle, the reward for a Storage Node is calculated based on the following parameters:

- Total Reward： The total amount of tokens accumulated in the reward pool at the end of the challenge cycle.
- InserviceSpace： The amount of storage space that the miner has actively used to serve users during the challenge.
- IdleSpace： The amount of verified idle storage space held by the miner during the challenge.
- TotalStoragePower： The total storage power of all miners who passed the challenges during that cycle.

<figure><img src="../assets/tokenomics/distribution.png" alt=""><figcaption><p> </p></figcaption></figure>
