# @cessnetwork/api

CESS Chain Interface Implementation in TypeScript.

## Installation

```bash
npm install @cessnetwork/api
```

## Pallets Overview

The CESS network consists of multiple pallets that provide different functionality for the decentralized storage system:

### System Pallet
The System pallet provides fundamental low-level functions and chain state management capabilities. It includes essential operations like account queries, block information retrieval, chain properties, and system health status. This pallet forms the foundation for all other pallets in the network.

### Balances Pallet
The Balances pallet handles token management and account balance operations. It enables users to transfer tokens, query account balances and locks, and perform batch transfers. The pallet also supports burning tokens and managing balance holds for various network operations.

### Staking Pallet
The Staking pallet manages the network's proof-of-stake consensus mechanism. It allows users to stake tokens, become validators or nominators, earn rewards, and participate in network governance. The pallet tracks stake amounts, validator performance, rewards distribution, and era-based operations.

### Sminer Pallet
The Sminer pallet manages miner operations and storage capacity in the CESS network. Miners can register, declare storage capacity, earn rewards, and participate in the proof-of-storage consensus mechanism. The pallet handles miner registration, staking, capacity management, and reward distribution.

### File Bank Pallet
The File Bank pallet manages file storage operations, including file uploads, downloads, and storage order management. It handles storage declarations, file metadata, restoral orders, and file availability proofs. This pallet is central to the decentralized storage functionality of CESS.

### Audit Pallet
The Audit pallet implements the proof-of-storage consensus mechanism through random challenges and proofs. It verifies that miners are correctly storing the data they committed to store using idle and service proofs. The pallet manages challenge snapshots and verification processes.

### TEE Worker Pallet
The TEE Worker pallet manages trusted execution environment workers that provide secure computation and verification services. These workers sign proofs and participate in secure verification processes to ensure data integrity and storage compliance.

### OSS Pallet
The OSS (Object Storage Service) pallet manages storage service providers and their authorization. It handles OSS node registration, authorization management, and proxy authentication, allowing service providers to interact with the network on behalf of users.

### Storage Handler Pallet
The Storage Handler pallet manages storage territories, pricing, and consignment operations. It handles territory creation, expansion, renewal, and transfer, as well as storage order processing and territory management functions.

### CESS Treasury Pallet
The CESS Treasury pallet manages network rewards and treasury funds. It tracks currency rewards, era rewards, and reserve rewards distributed to participants in the network.

### Session Pallet
The Session pallet manages validator sessions and helps maintain the active validator set that participates in block production and validation.

## Usage

### Basic Usage
```typescript
import { CESS, CESSConfig } from '@cessnetwork/api';

async function main() {
    const config: CESSConfig = {
        rpcs: ["wss://pm-rpc.cess.network/ws/"],
        privateKey: process.env.CESS_PRIVATE_KEY || "",
    }
    const cess = await CESS.newClient(config);

    console.log('Connected to network:', cess.getNetworkEnv());
    console.log('Chain Metadata:', cess.getMetadata());
    if(isKeyringReady(cess.keyring)){
        console.log('Account address:', cess.getSignatureAcc());
        console.log('Account balance:', cess.getBalances());
    }

    const accountInfo = await cess.queryAccountById("cXjrvyVimC51v8bTgB8SqvEsATRQYiEttYS8pRWDEjcurmvTS");
    console.log('Account Info:', accountInfo);

    const minerInfo = await cess.queryMinerByAccountId("cXjrvyVimC51v8bTgB8SqvEsATRQYiEttYS8pRWDEjcurmvTS");
    console.log(`Query Miner Info for cXjrvyVimC51v8bTgB8SqvEsATRQYiEttYS8pRWDEjcurmvTS result:`, minerInfo);

    await cess.close();
}

main().catch(console.error);
```