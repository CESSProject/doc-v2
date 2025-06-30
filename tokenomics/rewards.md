The CESS Network has strategically chosen to allocate a significant portion of its Total Token Supply (TTS) to mining rewards-an initiative aimed at strengthening its ecosystem. This allocation clearly reflects the network's recognition of the value contributed by its participants. The rationale behind this distribution is as follows:

**Consensus Node Operators**: 
As the backbone of the CESS Network, consensus node operators provide essential computing power, bandwidth, and storage. The incentive fund is designed to attract more operators and ensure their continued commitment.

**Storage Node Providers**: 
The network relies on storage nodes to supply storage resources, along with some bandwidth and computing power. By appropriately incentivizing them, CESS attracts the most efficient and reliable resource providers.

**CD²N Node Providers (Content Decentralized Delivery Network)**: 
A large number of decentralized CD²N nodes contribute bandwidth and caching resources, forming high-speed, cross-regional data delivery bridges between the CESS storage network and application endpoints. Incentivizing these nodes is key to building a more robust and efficient content delivery network.

In short, mining rewards are more than just a form of distribution-they are an investment. CESS is making a long-term commitment to its community, ensuring that the platform is not only viable today but remains strong and sustainable well into the future.

# Storage nodes
## Mining Rewards
Storage Nodes must pass random on-chain challenges in order to earn mining rewards. The amount of the reward depends on the reward pool for that challenge round and the proportion of the miner's storage power relative to the total network. Once rewards are issued, Storage Nodes must manually initiate a transaction to claim them.

The CESS Network uses a challenge cycle of every ***4 Eras*** (approximately 24 hours) to take snapshots of miners who have passed challenges, and rewards are calculated based on these snapshots. The reward pool for a given challenge cycle is sourced from a fixed number of tokens distributed over those 4 Eras. (Currently, each Era lasts about ***6 hours***, and around ***324,688 $CESS*** are issued per Era. This follows a halving every ***4 years*** rule, with a portion of each issuance allocated to the reward pool.)
During the k-th challenge of the N-th challenge cycle, the reward for a Storage Node is calculated based on the following parameters:

- Total Reward： The total amount of tokens accumulated in the reward pool at the end of the challenge cycle.
- InserviceSpace： The amount of storage space that the miner has actively used to serve users during the challenge.
- IdleSpace： The amount of verified idle storage space held by the miner during the challenge.
- TotalStoragePower： The total storage power of all miners who passed the challenges during that cycle.

$$
StoragePower_{k} = InserviceSpace_{k} \times 95\% + IdleSpace_{k} \times 5\%
$$
$$
Reward_{order_{k}} = TotalReward_{N} \times \frac{StoragePower_{k}}{TotalStoragePower_{N}}
$$

## Reward Release
When a Storage Node claims their reward, the amount is calculated based on the snapshot. Once the reward is determined, *50%* is released immediately, while the remaining *50%* is linearly released over the following *90 blocks* days (approximately 24 hours), with *1/90* of the remaining amount becoming available each block day.

The miner’s available reward on the i-th block day can be calculated accordingly:

$$
AvailableReward_i = (RewardOrder_¡ \times 50\%) + \sum_{t=0}^{i} \frac{RewardOrder_t \times 50\%}{90} 
$$

The Reward Order will be deleted once the entire amount has been distributed. Note that the *90-round* release schedule may be subject to change on the mainnet.

Once a miner's reward is calculated, it is stored in the Reward Order Pool. Miners must manually send a transaction to claim their rewards. They may choose to wait until the last and claim it all at once to save on transaction fees.

## Slashing Mechanism
Storage Nodes may be subject to slashing under certain conditions, where a portion of their staked assets is deducted. The slashing amount depends on the type of violation as well as the miner’s total Idle and Inservice Space. There are two types of slashing: Clear Punishment and Inservice Punishment.
## Clear Punishment
If a Storage Node fails to submit Proof of Idle Space during a random challenge, no slashing occurs, but it will also not receive any rewards for that challenge.

However, if a Storage Node fails to submit Proof of Inservice Space, the consequences depend on their mining history:
- For miners who have never received mining rewards, failing to submit Proof of Inservice Space during a random challenge results in a *5%* slashing of $CESS from their stake.
- For miners who have previously earned mining rewards, slashing is applied based on specific rules, taking into account the amount of Inservice Space involved.

$$
StorageSpace = IdleSpace + InserviceSpace
$$

$$
SlashLimit = \left(2000 ~ \$CESS/TiB\right) \times StorageSpace \times 5\% (in ~ TiB, round ~ up ~ to ~ integer)
$$

Even if the actual service space is less than 1 TiB, it will still be treated as 1 TiB for slashing calculations.

If a miner fails to submit Proof of Inservice Space five times in a row, they will be permanently removed from the storage network and banned from rejoining in the future.

## Inservice Punishment
If a Storage Node submits the Proof of Idle Space during a random challenge but the proof fails validation, no slashing occurs; however, no reward will be calculated for that challenge.

- If a miner fails Proof of Inservice Space validation once, there is no slashing.
- If a miner fails Proof of Inservice Space validation two or more consecutive times, staking penalties will be applied for each failure according to the following rules:

$$
StorageSpace = IdleSpace + InserviceSpace
$$

$$
SlashLimit = \left(2000 ~ \$CESS/TiB\right) \times StorageSpace \times 5\% (in ~ TiB, round ~ up ~ to ~ integer)
$$


If a miner successfully passes validation in a subsequent challenge round, all prior failure records are cleared, and the count of failed challenges resets.

There is no upper limit to the number of slashing events for Inservice Punishment. A miner may lose their entire stake over multiple consecutive failures, and may even fall into debt.

If a miner's staked amount falls below the required amount for their declared storage space:
- The miner will be placed in a "Frozen" state.
- Frozen miners cannot validate idle space or serve data, and cannot receive mining rewards.
- They will still be subject to random challenges, but will not be eligible for any rewards from them.

# Consensus Nodes
## Mining Rewards
For each Era, Consensus nodes receive proportional rewards based on the Era points they accumulate. Era points are earned in the following ways:
- Producing valid (canonical) blocks
- Including references to previously unreferenced Uncle Blocks
- Referencing Uncle Blocks that have already been included

Uncle Blocks are relay chain blocks that are valid in every aspect but failed to become canonical. This usually happens when two or more validators produce a block in the same slot, and one validator's block reaches the next block producer first. These delayed blocks are referred to as Uncle Blocks.

The reward calculation for Consensus nodes follows these rules:

$$
Reward = TotalReward_{Era} \times \frac{Points_{Era}}{TotalPoints_{Era}}
$$

Consensus nodes can manually claim rewards after each Era. Rewards can be accumulated for up to 84 Eras before needing to be claimed.

## Reward Release
Rewards are released at the end of each Era. Regardless of the amount staked, block production rewards are generally evenly distributed among all Consensus nodes.

However, as mentioned above, an individual miner's share may vary based on the Era points they earned.

While Era points involve an element of randomness and may be slightly affected by factors like network latency, well-performing Consensus nodes should accumulate similar amounts of Era points over time across many Eras.

Consensus nodes may also receive tips from transaction senders as an incentive for including transactions in the blocks they produce. Miners receive *20%* of these tips directly. The remaining *80%* goes into the treasury as a reserve fund.

## Slashing Mechanism
### Unresponsiveness Slashing
If a validator fails to produce any blocks during an Era and also does not send a Heartbeat Signal, they are flagged as "Unresponsive." Depending on the number of repeated offences and the overall number of other Unresponsive or offline validators during that ERA, the validator may be subject to slashing of their staked tokens.

Validators are expected to maintain a robust network infrastructure to ensure node uptime and minimize the risk of slashing or being placed into a cooldown period. It is recommended to implement high availability setups with backup nodes—but only activate a backup after confirming the primary node is offline, in order to avoid double-signing or other ambiguous behaviors that could trigger more severe penalties (see below):

```
 Let x = number of offending validators
 Let n = total number of validators in the active set
 Let MSA = Minimum Staking Amount
```

$$
 min \left(\frac{3 \times \left(x-(\frac{n}{10} +1)\right)}{n}, 1\right) \times 5\% \times MSA
$$

### Equivocation Slashing
Equivocation in both GRANDPA and BABE consensus mechanisms follows the same slashing formula:
- GRANDPA Equivocation: When a validator signs two or more conflicting votes in the same round.
- BABE Equivocation: When a validator produces two or more blocks in the same slot.
 Formula:
```
 Let x = number of offending validators
 Let n = total number of validators in the active set
 Let MSA = Minimum Staking Amount
```
$$
min \left((\frac{3 \times x}{n})^2, 1\right) \times 5\% \times MSA
$$

Validators can operate nodes on multiple machines to ensure redundancy. However, they must exercise extreme caution when managing these nodes. Improper configuration may result in equivocation, which is penalized more harshly than standard downtime or non-responsiveness.

If a validator is reported for any offence, they are removed from the active validator set (put into a cooldown period) and will not receive rewards while inactive.
They are immediately marked as Inactive and must re-declare their intent to validate in order to return to the active set.

# CD²N nodes
The CESS CD²N consists of a large and modular lineup of components, with two primary types of miner nodes：
- CD²N nodes offering enterprise-grade high-performance data upload and retrieval services (e.g., DeOSS)
- CD²N nodes providing content distribution services, such as lightweight caching nodes

In addition, the network includes lightweight client nodes, gateway nodes, and related components. CESS does not mandate specific functionalities for CD²N nodes. Instead, it issues rewards based solely on whether a node provides the required type of service, allowing developers to customize services freely according to their needs using the set of components CESS provides.

## Mining Rewards
The CESS CD²N network is designed to facilitate peer-to-peer value exchange between node providers and users, aiming to optimize the use of idle node resources. As such, CD²N node rewards are primarily derived from Retrieval and Caching Order Revenue.

To encourage early infrastructure development and healthy network growth, the reward release rules are as follows:
- For Retrieval Service Nodes, rewards are distributed based on the proportion of retrieval orders fulfilled by a node within each service cycle, relative to the total number of orders.

$$
Reward_{retrieval}=Income_{retrievalOrder}*95\%+\cfrac{RetrievalPower}{TotalRetrievalPower}*Reward_{serviceCycle}
$$

- For Caching Service Nodes, rewards are issued in the form of mining points, determined by the workload and share of caching tasks completed during each service cycle.

$$
Points_{cache}=BytesNum_{cacheOrder}*Point_{base}+\cfrac{CachedBytes}{TotalCachedBytes}*Point_{serviceCycle}
$$

Caching nodes can redeem their mining points for CESS token rewards.

## Reward Release
CD²N node rewards are released to each dedicated reward pool for nodes at the end of every service cycle. Nodes must manually initiate a transaction to claim their rewards.
- Each node can claim once per service cycle
- Each claim can only withdraw *50%* of the total available rewards
- The remaining *50%* can be claimed in future service cycles

### Slashing Mechanism
CESS implements a proactive incentive model for the CD²N network-no penalties or slashing are applied. Nodes are encouraged to earn more by working more", promoting positive participation without punitive deterrents.
