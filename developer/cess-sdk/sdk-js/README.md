# CESS SDK
CESS SDK implementation for TypeScript.

## Installation

```bash
npm install @cessnetwork/api @cessnetwork/util @cessnetwork/types
```

## Overview

## Complete Workflow with All Three Packages

This workflow demonstrates how all three packages work together:
- `@cessnetwork/api` provides the main client and gateway operations
- `@cessnetwork/util` provides essential utility functions like signing
- `@cessnetwork/types` provides all the type definitions needed for type safety

This section explains how to use all three CESS packages (`@cessnetwork/api`, `@cessnetwork/util`, and `@cessnetwork/types`) together to perform a complete gateway workflow for file storage operations on the CESS network.

### Step 1: Initialize CESS Client

```typescript
import { CESS, CESSConfig, downloadFile, ExtendedDownloadOptions, GenGatewayAccessToken, isKeyringReady, SDKError, uploadFile } from '@cessnetwork/api';
import { safeSignUnixTime, } from '@cessnetwork/util';
import { GatewayConfig, OssAuthorityList, OssDetail, UploadResponse } from "@cessnetwork/types";
import { u8aToHex } from "@polkadot/util";

const config: CESSConfig = {
    rpcs: ["wss://pm-rpc.cess.network/ws/"],
    privateKey: process.env.CESS_PRIVATE_KEY || "",
}
const cess = await CESS.newClient(config);

console.log('Connected to network:', cess.getNetworkEnv());
console.log('Chain Metadata:', cess.getMetadata());
if (isKeyringReady(cess.keyring)) {
    console.log('Account address:', cess.getSignatureAcc());
    console.log('Account balance:', cess.getBalances());
} else {
    throw new SDKError('Keyring Pair is required', 'INVALID_KEYRING');
}


```

### Step 2: Sign Message for Gateway Authentication

Using `@cessnetwork/util`'s signing utilities to authenticate with the gateway:

```typescript
const sign_message = Date.now().toString();
const signature = safeSignUnixTime(sign_message, getMnemonic());
console.log("signature:", signature);
```

### Step 3: Create Storage Territory (if needed)

```typescript
// Check if territory exists, if not create one
let territoryToken
const myTerritory = "default"
const territory = await cess.queryTerritory(accountAddress, myTerritory) as Territory;
const curBlockHeight = await cess.queryBlockNumberByHash()
console.log('Current Block Height:', curBlockHeight);

if (!territory) {
    try {
        const result = await cess.mintTerritory(
            1, // gibCount
            myTerritory,
            30, // days
        );

        if (result.success) {
            console.log('✅ Mint successful!', {txHash: result.txHash, blockHash: result.blockHash});

            // Wait and query the minted territory
            console.log('Waiting 6 seconds before querying territory...');
            await new Promise(resolve => setTimeout(resolve, 6000));

            const territory = await cess.queryTerritory(accountAddress, myTerritory) as Territory;
            territoryToken = territory?.token
            console.log('Territory details:', territory);
            console.log('Territory Token:', territoryToken);
        } else {
            console.error('❌ Territory minting failed:', result.error);
        }
    } catch (error) {
        console.error('❌ Error during territory minting:', error);
    }
} else if (territory.deadline - curBlockHeight <= 100800) {
    // 100800 block means 7 days
    const renewalRes = await cess.renewalTerritory(myTerritory, 10)
    console.log('renew territory successfully:', renewalRes.blockHash);
} else if (territory.state != "Active" || curBlockHeight >= territory.deadline) {
    // data will be reset when re-activate territory
    const reactivateRes = await cess.reactivateTerritory(myTerritory, 30)
    console.log('reactivate territory successfully:', reactivateRes.blockHash);
} else if (territory.remainingSpace <= 1024 * 1024 * 1024) {
    // remaining space <= 1GiB
    const expandRes = await cess.expandingTerritory(myTerritory, 1)
    console.log('expand territory successfully:', expandRes.blockHash);
} else {
    console.log("territory exist: ", territory)
}
```

### Step 4: Authorize Gateway Access

```typescript
let gatewayAcc = ""
// get oss public acc by domain name
const ossAccList = await cess.queryOssByAccountId() as unknown as OssDetail[]
for (let i = 0; i < ossAccList.length; i++) {
    if (ossAccList[i].ossInfo.domain == gatewayUrl) {
        gatewayAcc = ossAccList[i].account
        break;
    }
}
if (!gatewayAcc) {
    console.error("gateway not found.");
    return;
}

// auth my territory to gateway acc
const authList = await cess.queryAuthorityListByAccountId(gatewayAcc) as unknown as OssAuthorityList[]
const authorizedAccounts: string[] = authList.map(item => item.authorizedAcc);
if (!authorizedAccounts.indexOf(accountAddress)) {
    try {
        console.log("start to auth")
        const result = await cess.authorize(gatewayAcc);
        if (result.success) {
            console.log('✅ Authorization successful!', {txHash: result.txHash, blockHash: result.blockHash});
        } else {
            console.error('❌ Authorization failed:', result.error);
        }
    } catch (error) {
        console.error('❌ Error during authorization:', error);
    }
} else {
    console.log("already auth")
}
```

### Step 5: Obtain Gateway Token

```typescript
const token = await GenGatewayAccessToken(gatewayUrl, {
    account: cess.getSignatureAcc(),
    message: sign_message,
    sign: u8aToHex(signature), // Convert signature to hex format
    expire: 1 // Token expiration in hours
});

```

### Step 6: Upload File to Gateway

```typescript
// Configure gateway connection
const gatewayConfig: GatewayConfigType = {
    baseUrl: gatewayUrl,
    token: token
};

// Upload file to the gateway
let uploadResult: UploadResponse;
uploadResult = await upload(gatewayConfig, "./path/to/your/file.txt", {
    territory: myTerritory,
    uploadFileWithProgress: (progress) => {
        console.log(`\rUpload progress: ${progress.percentage}% (${progress.loaded}/${progress.total} bytes) - ${progress.file}`);
    }
});
if (uploadResult.code == 200) {
    const fid = uploadResult.data; // File ID returned by the gateway
    console.log("File ID (FID): ", fid);
} else {
    throw new Error("Failed to upload file: " + uploadResult.error);
}
```

### Step 7: Download File from Gateway

```typescript
// Define download options
const downloadOptions: ExtendedDownloadOptions = {
    fid: fid, // The file ID from the upload step
    savePath: "./path/to/downloaded/file.txt",
    overwrite: true,
    createDirectories: true,
};

// Download the file from the gateway
const downloadResult = await downloadFile(gatewayConfig, downloadOptions);
if (downloadResult.success) {
    console.log("Download result: ", downloadResult.data);
} else {
    throw new Error("Failed to download file: " + (downloadResult.error || "Unknown error"));
}
console.log("Download result:", downloadResult);
```

### Step 8: Query File Status on the Blockchain

```typescript
// Check if the file is being processed by storage miners or already stored
const dealMap = await cess.queryDealMap(fid);
if (dealMap) {
    console.log("Storage order details: ", dealMap);
    console.log("File is being distributed to storage miners");
} else {
    console.log("File has been stored on storage nodes");
    const fileMeta = await cess.queryFileByFid(fid);
    console.log("File metadata: ", fileMeta);
}

await cess.close()
```
