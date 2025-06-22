# Consensus Nodes
## Staking for Mining
Each consensus node must stake at least 3,000,000 $CESS to be eligible for election as a validator. This stake can be raised through nominators, with each nominator account required to stake a minimum of 100 $CESS. If the total stake falls below 3,000,000 $CESS, miners can add more stake to maintain validator candidacy.
## Stake Redemption
When a consensus node initiates an exit transaction through their Stash account, a cooling-off period of approximately 112 Eras (1 Era = 6 hours, equivalent to approximately 28 days) is required. During this time, the staked funds remain frozen. Once the cooling period ends, the staked tokens are unlocked and ready for withdrawal.
# Storage Nodes
## Staking for Mining
Storage Nodes on the CESS Network use a linear staking model. A stake of 2,000 $CESS is required per TiB of declared storage space. Note that the minimum storage unit is 1 TiB, meaning at least 2,000 $CESS must be staked to participate.

If a Storage Node’s stake falls below the amount required for their declared capacity, the miner will be placed in a “Frozen” state and will not earn any mining rewards. They can return to an “Active” state by increasing their stake.

If a miner falls into debt (i.e., penalties exceed the staked amount), any additional stake will first be used to repay the debt.

When a Storage Node expands its declared capacity, the new stake resets the Staking period, and any future redemption will be based on this new period.

## Stake Redemption
If a Storage Node is forcibly removed due to penalty mechanisms and has never earned mining rewards, they may immediately redeem their remaining staked tokens. This protection mechanism allows miners to trial the system without the risk of unexpected asset loss.

For Storage Nodes that have already earned rewards, any unclaimed rewards may be immediately claimed upon forced removal, while unreleased rewards will be slashed. Remaining staked tokens can be redeemed after the Staking period ends (180 block days).

If a Storage Node voluntarily exits, they must first enter a 15-block-day lock period during which they may face random challenge checks. Failing these challenges results in penalties. During the lock period, miners cannot receive mining rewards. After the lock period ends:

- If the miner is still in the "Active" state, they will enter a 15-block-day Backup period.

- If in the "Frozen" state, the exit request is rejected, and the process must be restarted.

Once the Backup and Staking periods are completed, the miner can normally redeem their stake and claim any unclaimed rewards. Unreleased rewards will continue to be distributed linearly over the remaining release schedule.
