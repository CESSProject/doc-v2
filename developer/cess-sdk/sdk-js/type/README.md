# @cessnetwork/types

CESS Type definitions for CESS SDK.

## Installation

```bash
npm install @cessnetwork/types
```

## Overview

This package provides TypeScript type definitions for the CESS (Content Edge Storage Space) blockchain SDK. It includes interfaces, types, and HTTP response types for various pallets and modules used in the CESS network.

## Exports

This package exports type definitions organized into three main categories:

### 1. Core Types (`./types`)
- Common utility types and constants
- Type aliases for blockchain primitives
- Event interfaces

### 2. Pallet Types (`./pallets`)
- **Audit**: Types for auditing and proof verification (ChallengeInfo, MinerSnapShot, SpaceProof, etc.)
- **Balances**: Types for token and balance management
- **File Bank**: Types for file storage and management (FileMetadata, BucketInfo, etc.)
- **OSS**: Types for Object Storage Service integration
- **RPC**: Types for Remote Procedure Call interfaces
- **Sminer**: Types for mining operations (MinerInfo, MinerLedger, etc.)
- **Staking**: Types for staking and validator operations
- **Storage Handler**: Types for storage management operations
- **System**: System-level types
- **TEE**: Types for Trusted Execution Environment operations

### 3. HTTP Types (`./http`)
- Common HTTP response types
- Gateway API response types
- Sminer-related HTTP response types

## Usage

```typescript
import { 
  // Core types
  AccountIdInput, 
  BalanceInput,
  TokenPrecision_CESS,
  
  // Audit types
  ChallengeInfo, 
  MinerSnapShot,
  SpaceProofInfo,
  
  // Sminer types
  MinerInfo,
  
  // File Bank types
  FileMetadata,
  
  // Event types
  Event_UploadDeclaration,
  
  // HTTP types
  GatewayResponse
} from '@cessnetwork/types';

// Use the types in your application
const challenge: ChallengeInfo = {
  minerSnapshot: {
    idleSpace: BigInt(1000000),
    serviceSpace: BigInt(500000),
    serviceBloomFilter: [],
    spaceProofInfo: {
      miner: "cX...",
      front: 0,
      rear: 10,
      poisKey: { g: "0x...", n: "0x..." },
      accumulator: "0x..."
    },
    teeSignature: "0x..."
  },
  challengeElement: {
    start: Date.now(),
    idleSlip: 1,
    serviceSlip: 1,
    verifySlip: 1,
    spaceParam: [],
    serviceParam: {
      randomIndexList: [1, 2, 3],
      randomList: ["0x...", "0x..."]
    }
  },
  proveInfo: {
    assign: 1,
    idleProve: null,
    serviceProve: null
  }
};

// Common input types for API parameters
const accountId: AccountIdInput = "cXhT9Xh3DhrBMDmXcGeMPDmTzDm1J8vDxBtKvogV33pShnWS";
const balance: BalanceInput = BigInt(1000000000000000000); // 1 CESS token
```

## Common Constants and Utilities

The package also exports useful constants and utilities:

```typescript
import { 
  TokenPrecision_CESS, 
  StakeForOneTB, 
  BlockInterval, 
  Size, 
  AddressSs58Format,
  NumberOfDataCopies,
  SegmentSize,
  FragmentSize
} from '@cessnetwork/types';

// Token precision for CESS (10^18)
console.log(TokenPrecision_CESS); // 1000000000000000000n

// Size constants
console.log(Size.SIZE_1GiB); // 1073741824

// Address format for mainnet and testnet
console.log(AddressSs58Format.MAIN_NET); // 11331
```

## Documentation

For more detailed documentation, please visit [CESS official documentation](https://doc.cess.network/developer/cess-sdk/javascript-sdk).

## License

This project is licensed under the Apache-2.0 License.