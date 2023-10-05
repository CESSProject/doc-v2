# Before We Start

In this section we will give a mid-level description of the source code and architecture of CESS blockchain. It will mainly revolved around the repository [**CESSProject/cess**](https://github.com/CESSProject/cess). We say this is a "mid-level" description meaning it is more details than just overall architecture description. It will get down to the source code to get general understanding how different modules are connected together, but it is not so low-level and we will NOT go through the source code line by line. You don't need the knowledge in this page to use CESS products, but if you are a developer and want to get a deeper understanding on how CESS blockchain works, this section is for you.

# Overview

- It can be easily understood by the following diagram.

![insert-diagram here](insert-diagram-here)

- CESS blockchain is built on top of Parity developed Substrate blockchain SDK. So we adopt a similar architecture of Substrate-based blockchain.
- An overall architecture of Substrate-based blockchain:
  - Node client: it manage the p2p library network, the JSON-RPC api that interface with external work, etc...
  - Runtime: the business logic of the blockchain. It is composed of multiple pallets (also known as modules) connecting together
  - Pallets: the basic building block for runtime. It specify the on-chain storage, events, errors, and the state transition functions. A blockchain runtime is built by putting one or more pallets together in the runtime.

# High Level Understanding

Versions we are using:
- CESSProject/cess [v0.7.3](https://github.com/CESSProject/cess/tree/0.7.3)
- CESSProject/substrate [cess-polkadot-v0.9.42](https://github.com/CESSProject/substrate/tree/cess-polkadot-v0.9.42)

Now running a CESS node is cloning [**CESSProject/cess**](https://github.com/CESSProject/cess), build it, and run the executable. We can mainly understand what CESS node does by looking at how the runtime is constructed in [`runtime/src/libs.rs`](https://github.com/CESSProject/cess/blob/0.7.3/runtime/src/lib.rs):

```rs
construct_runtime!(
  pub enum Runtime where
    Block = Block,
    NodeBlock = opaque::Block,
    UncheckedExtrinsic = UncheckedExtrinsic
  {
    // Basic stuff
    System: frame_system = 0,
    RandomnessCollectiveFlip: pallet_randomness_collective_flip = 1,
    Timestamp: pallet_timestamp = 2,
    Sudo: pallet_sudo = 3,
    Scheduler: pallet_scheduler = 4,
    Preimage: pallet_preimage = 5,
    Mmr: pallet_mmr = 6,

    // Account lookup
    Indices: pallet_indices = 7,

    // Tokens & Fees
    Balances: pallet_balances = 10,
    TransactionPayment: pallet_transaction_payment = 11,
    Assets: pallet_assets = 12,
    AssetTxPayment: pallet_asset_tx_payment = 13,

    // Consensus
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

    // Governance
    Council: pallet_collective::<Instance1> = 40,
    TechnicalCommittee: pallet_collective::<Instance2> = 41,
    TechnicalMembership: pallet_membership::<Instance1> = 42,
    Treasury: pallet_treasury = 43,
    Bounties: pallet_bounties = 44,
    ChildBounties: pallet_child_bounties = 45,

    // Smart contracts
    Contracts: pallet_contracts = 50,
    Ethereum: pallet_ethereum = 51,
    EVM: pallet_evm = 52,
    DynamicFee: pallet_dynamic_fee = 53,
    BaseFee: pallet_base_fee = 54,

    // CESS pallets
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

These are the different pallets that are composed in CESS node runtime, grouped in category. We will give a two-sentence descriptions to each of the pallet above, except for pallet in the last categories, those implemented specifically for CESS blockchain.

The first category is the foundation of the blockchain. They ...

- **System**: The System pallet provides low-level access to core types and cross-cutting utilities. It acts as the base layer for other pallets to interact with the Substrate framework components. This pallet implements functions such as getting the current block number, getting the parent hash of the current block, etc. [Docs](https://paritytech.github.io/polkadot-sdk/master/frame_system).
- **RandomnessCollectiveFlip**: The Randomness Collective Flip pallet provides a random function that generates low-influence random values based on the block hashes from the previous 81 blocks. It is a pseudo random number generator on the blockchain. [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_insecure_randomness_collective_flip).
- **Timestamp**: A pallet for setting on-chain time, recorded in unix timestamp. [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_timestamp).
- **Sudo**: A pallet to authenticate the request as a `root` user. [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_sudo).
- **Scheduler**: A pallet for scheduling runtime calls. [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_scheduler).
- **Preimage**: [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_preimage).
- **Mmr**: [Docs](https://paritytech.github.io/polkadot-sdk/master/pallet_mmr)

The second category is on accounts, tokens and fees.

- **Indices**:
- **Balances**:
- **TransactionPayment**:
- **Assets**:
- **AssetTxPayment**:

The third category is on consensus. They includes from Authorship to ElectionProviderMultiPhase pallets.

The forth category is on DAO and governance related.

The fifth category is on both ink! and EVM-compatible smart contract.

The sixth category are the pallets that our core development team built. These pallets include:

- **FileBank**: This pallet contains operations related info of files on multi-direction. Provide storage space purchase, capacity expansion and lease renewal interfaces. Including actions in CRUD user storage buckets (similar to a directory), and user files. The actual files and buckets are created in the storage network, but the action and its metadata are all stored on the blockchain.

- **TeeWorker**: This pallet provides functionality for managing the consensus miner. It includes registering a node to be a miner, updating the whitelist of the miner, and for miner to exit from the consensus mining queue.

- **Audit**: This pallet manage the proof of PoDR2 adaptation. It processes the proof of miner's service file and filling file, and generate random challenges. Call some traits of Smith pallet to punish miners. It call the trait of file bank pallet to obtain random files or files with problems in handling challenges.

- **Sminer**: This pallet contains operations related storage miners, allowing a storage miners to claim how much space it is providing for how long, staking its token for its claimed services, and withdrawing the service provision altogether.

- **StorageHandler**: This pallet manage and perform the book-keeping of the total storage space provided by the underlying storage miners. It allows users to rent, expand the space. Update the pricing of the space. It will also emit events when the space leasing period is ending.

- **SchedulerCredit**: The consensus miner credit module. The scheduler reputation module is used to record some operations performed by the scheduler in order to maintain the chain state, including records of processing files and records of penalties for failing to achieve scheduling goals. Finally, the system will give a score to the scheduler according to the set algorithm. This score will be used to decide validators as part of the R2S consensus.

- **Oss**: This pallet records the information about OSS, which stands for Object Storage System. It allows for registration, update, and marking removal of OSS.

- **Cacher**: This pallet provide functionality about cache miners. Cache miner retrieves the bills in the transaction records and provides file downloading service. It provide the function to register a new cache miner, update its information, logging out of the miner, and payment to the cache miner.

If you look at the file [`runtime/Cargo.toml`](https://github.com/CESSProject/cess/blob/0.7.3/runtime/Cargo.toml), you will see that a lot of pallets are dependencies coming from the project `CESSProject/substrate`. They are mostly pre-built in the Substrate framework with minor customization. We have added a handful of pallets (pallets in the sixth category) and these are pallets that make us unique. In the following section, we will go in and see what each of these pallets does.

# Pallets

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

## Audit Pallet

## Sminer Pallet

## StorageHandler Pallet

## SchedulerCredit Pallet

## OSS Pallet

## Cacher Pallet
