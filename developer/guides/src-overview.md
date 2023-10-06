# Before We Start

In this section we will give a mid-level description of the source code and architecture of CESS blockchain. It mainly revolves around the repository [**CESSProject/cess**](https://github.com/CESSProject/cess). This is a "mid-level" description meaning it is more details than just a high level description. Some part of the documentation it should be read with the source code open next to it. You will get a general understanding on how different modules are connected together, but it is not so low-level that we go through the source code line by line. You don't need the knowledge in this page to use CESS products, but if you are a developer and want to gain a deeper understanding on how CESS blockchain works, this section is for you.

# Overview

CESS blockchain is built on top of Parity developed Substrate blockchain SDK. So we adopt a similar architecture of Substrate-based blockchain. It can be easily understood by the following diagram.

![Substrate Architecture](../../assets/developer/guides/src-overview)

In the outer layer there is a Node (a.k.a the Client). It contains all the low-level building blocks of a blockchain, such as the peer-to-peer network, JSON-RPC API that interface externally, and the storage and database system.

Then inside the node, there is a **Runtime**, where the chain logic happens. This is where user accounts are being recognized and their balances are being stored. Substrate-based blockchain also have a strong sense of DAO and have a comprehensive way of raising a proposal and passing them through a referendum or a council. These functionality are all specified in the chain runtime.

As complicated of the features provided by the runtime. It is actually further broken down into pallets. One, or sometimes a few, of the pallets implement one set of functionality of the runtime. As these pallets are independent and modular, once they are imeplemented, it is up to the engineer of the Runtime to compose these pallets goether to provide the functionality of the blockchain.

With this architecture, CESS blockchain, and extending to most Substrate-based blockchains are modular, composible, and scalable.

# High Level Understanding

To understand what a CESS blockchain does, we can look at its Runtime and see what pallets is it composed of. The CESS blockchain version we are looking at is:

CESSProject/cess [v0.7.3](https://github.com/CESSProject/cess/tree/0.7.3)

Let's look at its runtime [`runtime/src/libs.rs`](https://github.com/CESSProject/cess/blob/0.7.3/runtime/src/lib.rs), and especially at the section of code in `construct_runtime!()`:

```rs
construct_runtime!(
  pub enum Runtime where
    Block = Block,
    NodeBlock = opaque::Block,
    UncheckedExtrinsic = UncheckedExtrinsic
  {
    // First section - foundational pallets
    System: frame_system = 0,
    RandomnessCollectiveFlip: pallet_randomness_collective_flip = 1,
    Timestamp: pallet_timestamp = 2,
    Sudo: pallet_sudo = 3,
    Scheduler: pallet_scheduler = 4,
    Preimage: pallet_preimage = 5,
    Mmr: pallet_mmr = 6,

    // Second section - Account, balances, and transaction fees
    Indices: pallet_indices = 7,
    Balances: pallet_balances = 10,
    TransactionPayment: pallet_transaction_payment = 11,
    Assets: pallet_assets = 12,
    AssetTxPayment: pallet_asset_tx_payment = 13,

    // Third section - Managing Session, Validators, Block Production, and Block Finalization
    Authorship: pallet_authorship = 20,
    Babe: pallet_rrsc = 21,  // retain Babe alias for Polka-dot official browser
    Grandpa: pallet_grandpa = 22,
    Staking: pallet_cess_staking = 23,
    Session: pallet_session = 24,
    Historical: pallet_session_historical = 25,
    Offences: pallet_offences = 26,
    ImOnline: pallet_im_online = 27,
    AuthorityDiscovery: pallet_authority_discovery = 28,
    VoterList: pallet_bags_list = 29,
    ElectionProviderMultiPhase: pallet_election_provider_multi_phase = 30,

    // Forth section - Chain Governance
    Council: pallet_collective::<Instance1> = 40,
    TechnicalCommittee: pallet_collective::<Instance2> = 41,
    TechnicalMembership: pallet_membership::<Instance1> = 42,
    Treasury: pallet_treasury = 43,
    Bounties: pallet_bounties = 44,
    ChildBounties: pallet_child_bounties = 45,

    // Fifth section - Smart contracts capability
    Contracts: pallet_contracts = 50,
    Ethereum: pallet_ethereum = 51,
    EVM: pallet_evm = 52,
    DynamicFee: pallet_dynamic_fee = 53,
    BaseFee: pallet_base_fee = 54,

    // Sixth section - CESS specific pallets
    FileBank: pallet_file_bank = 60,
    TeeWorker: pallet_tee_worker = 61,
    Audit: pallet_audit = 62,
    Sminer: pallet_sminer = 63,
    StorageHandler: pallet_storage_handler = 64,
    SchedulerCredit: pallet_scheduler_credit = 65,
    Oss: pallet_oss = 66,
    Cacher: pallet_cacher = 67,
  }
);
```

As seen from the above, there are over 40 pallets being integrated in the runtime. They can be broadly split into six groups.

1. Foundation Pallets - These pallets provide low-level access to core types and cross-cutting utilities. It acts as the base layer for other pallets to interact with the Substrate framework components. This includes keeping track of the chain block number, getting the parent hash of the current block, etc. These pallets include System, RandomnessCollectiveFlip, Timestamp, Sudo, Schedyler, Preimage, and MMR pallets.

2. Account, Balance and Fee Pallets - These pallets keep track of the existing accounts in the blockchain and their balances. For a user balance, there are both free and locked balances. It also keep track of other tokens and NFTs, they are accounted as assets in CESS. There are also pallet for handling transaction fee. These pallets include Indices, Balances, TransactionPayment, Assets, and AssetTxPayment pallets.

3. Consensus Pallets - These pallets work together to keep track of session in the blockchain, and within each session the corresponding validators and who author blocks. It also keep track of the votes by these validators to finalize blocks. In summary these pallets work together to ensure the block data is valid. These pallet includes Authorship, Babe, Grandpa, Staking, Session, Historical, Offences, ImOnline, AuthorityDiscovery, VoterList, and ElectionProviderMultiPhase pallets.

4. Governance Pallets - These pallets provide function to keep track of the Treasury. Some of these treasury are allocated to various proposals voted through council and committees. Some of the treasury allocation are one-off bounty (and sub-boundy) asking for solution providers for solutions and the committee member to vet the solutions. Together these pallets manage the mechanism of how the chain Treasury are spent. These pallets include Council, TechnicalCommittee, TechnicalMembership, Treasury, Bounties, and ChildBounties pallets.

5. Smart Contract Pallets - These pallets grand the chain functionality of running smart contract. CESS chain support two type of smart contracts - ink! contracts, this is granted by Contracts pallets; and EVM-compatible contracts, granted by Ethereum, EVM, DynamicFee pallets. Some EVM precompiles are also coded to allow Substrate chains recognized these built-in EVM functions.

6. CESS specific Pallets - These are pallets built by CESS core development team to implement CESS functionality. Most of the pallets on the first five categories are provided as-is in the Substrate framework. We have made further customization to make it suit CESS blockchain. But the pallets in this category is what make CESS blockchain unique and implement the core functionality of providing the decentralized storage and caching functionality.

Next, let's go deeper in these CESS-specific pallets.

# CESS Pallets

## FileBank Pallet

This pallet contains operations related info of files on multi-direction. Provide storage space purchase, capacity expansion and lease renewal interfaces. Including actions in CRUD user storage buckets (similar to a directory), and user files. The actual files and buckets are created in the storage network, but the action and its metadata are all stored on the blockchain.

Implementation: [`c-pallets/file-bank/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/file-bank/src/lib.rs)

Extrinsics:

- `upload_declaration()`: For users to declare a file is going to be upload before actual operation. It will check if the file is already on-chain and mark the corresponding metadata.
- `deal_reassign_miner()`: The deal hash is reassigned to another storage miner.
- `ownership_transfer()`: Transfer the ownership of a document. It will release the used space of the original owner and deduct the available space for the new owner.
- `transfer_report()`: This upload the information of a stored file. The same file will only upload meta information once, which will be uploaded by consensus.
- `calculate_end()`: For a deal with particular hash, we ends its deal, clean up the system storage, and finally remove the deal from `DealMap` storage.
- `replace_idle_space()`: **todo**
- `delete_file()`: Delete the file owned by the owner.
- `cert_idle_space()`: Upload up to ten idel files to certify the storage miner space.
- `create_bucket()`: Create a storage bucket.
- `delete_bucket()`: Delete a storage bucket.
- `generate_restoral_order()`: Generate a restoral order to be picked up by storage miner.
- `claim_restoral_order()`: Storage miner come and claim to perform a restoral order.
- `claim_restoral_noexist_order()`: **todo**
- `restoral_order_complete()`: Claiming that the restoral order is complete by the storage miner who claim the order.
- `root_clear_failed_count()`: Clear the failed count of all miners. Can only be called by `root`.
- `miner_clear_failed_count()`: Clear the failed count of its own as a miner.

## TeeWorker Pallet

This pallet provides functionality for managing the consensus miner. It includes registering a node to be a miner, updating the whitelist of the miner, and for miner to exit from the consensus mining queue.

Implementation: [`c-pallets/tee-worker/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/tee-worker/src/lib.rs)

Extrinsics:

- `register()`: Register for a consensus miner.
- `update_whitelist()`: Adding a new Enclave ID into the whitelist storage.
- `exit()`: Call when a consensus miner want to exit its role.

## Audit Pallet

This pallet manage the proof of PoDR2 adaptation. It processes the proof of miner's service file and filling file, and generate random challenges. Call some traits of Smith pallet to punish miners. It call the trait of file bank pallet to obtain random files or files with problems in handling challenges.

Implementation: [`c-pallets/audit/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/audit/src/lib.rs)

Extrinsics:

- `save_challenge_info()`: Storing a new challenge proposal on-chain.
- `submit_idle_proof()`: The storage miner submits an idle proof on-chain. This will also satisfy the challenger request who ask the miner for a proof.
- `submit_service_proof()`: The storage miner submits a service proof on-chain. This will also satisfy the challenger request who ask the miner for a proof.
- `submit_verify_idle_result()`:
- `submit_verify_service_result()`:

## Sminer Pallet

This pallet contains operations related storage miners, allowing a storage miners to claim how much space it is providing for how long, staking its token for its claimed services, and withdrawing the service provision altogether.

Implementation: [`c-pallets/sminer/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/sminer/src/lib.rs)

Extrinsics:

- `regnstk()`: extrinsic responsible for registering and staking of a new storage miner.
- `increase_collateral()`: Increasing the storage miner collateral (aka stake).
- `update_beneficiary()`: Update the beneficiary account of the storage miner.
- `update_peer_id()`: Update the storage miner peer ID.
- `receive_reward()`: The storage miner requesting reward to be issued.
- `miner_exit_prep()`: The storage miner request to exit in future.
- `miner_exit()`: The storage miner is exiting.
- `mine_withdraw()`: The stroage miner withdrawing. (what is the difference between exiting and withdrawing?)
- `faucet_top_up()`: The caller returning some of his holding token to the faucet.
- `faucet()`: The caller ask to receive from token from the faucet.

## StorageHandler Pallet

This pallet manage and perform the book-keeping of the total storage space provided by the underlying storage miners. It allows users to rent, expand the space. Update the pricing of the space. It will also emit events when the space leasing period is ending.

Implementation: [`c-pallets/storage-handler/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/storage-handler/src/lib.rs)

Extrinsics:

- `buy_space()`: Extrinsics for a user to purchase space.
- `expansion_space()`: Purchase additional space. It can only be called when `buy_space()` have been called by the user. The upgrade target also need to be larger than the existing purchased space.
- `renewal_space()`: Extend the lease of the current storage package for a specified days.
- `update_price()`: Update the base price of the storage. Can only be called by `root` user.
- `update_user_life()`: Update the user storage lifespan. Can only be called by `root` user.

## SchedulerCredit Pallet

The consensus miner credit module. The scheduler reputation module is used to record some operations performed by the scheduler in order to maintain the chain state, including records of processing files and records of penalties for failing to achieve scheduling goals. Finally, the system will give a score to the scheduler according to the set algorithm. This score will be used to decide validators as part of the R2S consensus.

Implementation: [`c-pallets/scheduler-credit/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/scheduler-credit/src/lib.rs)

Extrinsics: No callable extrinsics.

## OSS Pallet

This pallet records the information about OSS, which stands for Object Storage System <What is OSS stands for?>. It allows for registration, update, and marking removal of OSS.

Implementation: [`c-pallets/oss/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/oss/src/lib.rs)

Extrinsics:

- `authorize()`: Setting a specified operator for the caller. It can be understood similar as the caller delegating his rights to the operator.
- `cancel_authorize()`: Cancel the authorization of the operator on behalf of the caller.
- `register()`: Register the caller with a particular Peer ID.
- `update()`: Update the caller with a new Peer ID.
- `destroy()`: Remove the caller as the OSS.

## Cacher Pallet

This pallet provide functionality about cache miners. Cache miner retrieves the bills in the transaction records and provides file downloading service. It provide the function to register a new cache miner, update its information, logging out of the miner, and payment to the cache miner.

Implementation: [`c-pallets/cacher/src/lib.rs`](https://github.com/cessProject/cess/blob/main/c-pallets/cacher/src/lib.rs)

Extrinsics:

- `register()`: Register the caller as a cache provider.
- `update()`: Update the cache information about the caller. The caller must have registered as a cache provider previously.
- `logout()`: The caller is exiting as a cache provider.
- `pay()`: Paying all cache providers of payment due to them.
