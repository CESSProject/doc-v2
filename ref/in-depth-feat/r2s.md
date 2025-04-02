Consensus is a mechanism for coming to an agreement over a shared state. In order for the state of the blockchain to continue to build and move forward, most nodes in the network should agree and come to consensus. It is the way that the nodes in a decentralized network are able to stay synced with each other. Without consensus for the decentralized network of nodes in a blockchain, there is no way to ensure that the state one node believes is true will be shared and verified by the other nodes. Consensus aims to provide the objective view of the state amid participants who each have their own subjective views of the network. It is the process by which these nodes communicate and come to an agreement with each other, which enable the building of new blocks.

# Validator Selection: Random Rotation Selection

CESS uses the R²S (random rotation Selection) as its mechanism to select the validator nodes. Participants interested in maintaining the network can run the validator node. The validator plays the role of producing new blocks, verifying blocks and ensuring finality. Refer to [**Random Rotation Consensus**](./rrc.md) for details.

# Block Production: BABE

BABE (**B**lind **A**ssignment for **B**lockchain **E**xtension) is the block production mechanism that runs among the validator nodes and determines block producers. The chains' runtime is required to provide the BABE authority lists and randomness via a consensus message in the header of the first block of each epoch.

BABE execution happens in sequential non-overlapping phases known as epochs. Each epoch is divided into a predefined number of slots. All slots in each epoch are sequentially indexed starting from 0 (slot number). At the beginning of each epoch, the BABE node needs to run the Block-Production-Lottery algorithm to find out in which slots it should produce a block and gossip to the other block producers.

Validators participate in a lottery for every slot, which will inform whether or not they are the block producer candidate for that slot. Slots are discrete units of time of approximately 6 seconds in length. Because the mechanism of allocating slots to validators is based on a randomized design, multiple validators could be candidates for the same slot. In other times, a slot could be empty, resulting in inconsistent block time.

## Multiple Validators in a Slot

When multiple validators are block producer candidates in a given slot, all of them will produce a block and broadcast it to the network. At that point, it's a race. The validator whose block firstly are verified by the most of the network wins. Depending on network topology and latency, both chains will continue to build in some capacity, until the finalization kicks in and amputates a fork.

## No Validators in a Slot

It is possbile that there's no validator candidate been selected if every validators missed the randomness lottery to qualify for block production. If that happens, a slot would remain seemingly blockless. We avoid this by running a secondary, round-robin style validator selection algorithm in the background. The validators selected to produce blocks through this algorithm always produce blocks, but these secondary blocks are ignored if the same slot also produces a primary block from a VRF-selected validator. Thus, a slot can have either a primary or a secondary block, and no slots will be skipped.

# Block Finality: GRANDPA

GRANDPA (**G**host-based **R**ecursive **AN**cestor **D**eriving **P**refix **A**greement) is the finality gadget for CESS.

Finality is obtained by consecutive rounds of voting by the validator nodes. Validators execute GRANDPA finality process in parallel to block production as an independent service.

It works in a partially synchronous network model as long as 2/3 of nodes are honest and can cope with 1/5 Byzantine nodes in an asynchronous setting.

A notable distinction is that GRANDPA reaches agreements on chains rather than blocks, greatly speeding up the finalization process, even after long-term network partitioning or other networking failures. In other words, as soon as more than 2/3 of validators attest to a chain containing a certain block, all blocks leading up to that one are finalized at once.
