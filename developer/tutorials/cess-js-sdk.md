# Overall

In this tutorial, we will interact with the CESS testnet via [**cess-js-sdk**](https://www.npmjs.com/package/cess-js-sdk), so this is particularly useful for Node.js or frontend developers that want to incorporate CESS into their products/systems.

# Prerequisite

- [Node.js](https://nodejs.org/en) v18 or above.

# Code

The code used in this tutorial is in [**cess-course** Github repo](https://github.com/CESSProject/cess-course/tree/main/examples/js). We will walk through the code together.

The tutorial code performs the following:

1. Check the amount of my rented space and its expiration time on CESS (read operations on the blockchain).
2. Rent space on CESS testnet (write operations on the blockchain)
3. Create a bucket (write operations on the blockchain)
4. Query my existing files in the CESS Cloud (read operations on the blockchain)
5. Upload a file to the bucket (write operations on the blockchain)
6. Download the file back.

{% hint style="info" %}

**CESS Testnet RPC endpoint**: <wss://testnet-rpc0.cess.cloud/ws/><br/>
**CESS Testnet File gateway**: <https://deoss-pub-gateway.cess.cloud/>

{% endhint %}

First, install `cess-js-sdk` by running the following command.

```bash
pnpm add cess-js-sdk
# or
yarn add cess-js-sdk
# or
npm add cess-js-sdk
```

These commands add **cess-js-sdk** dependency into your project.

Then we initialize the API with `InitAPI()` call:

```ts
import {
  InitAPI,
  Space,
  Bucket,
  File,
  testnetConfig
} from "cess-js-sdk";

async function main() {
  const { api, keyring } = await InitAPI(testnetConfig);
  const space = new Space(api, keyring);
  const bucket = new Bucket(api, keyring);
  const file = new File(api, keyring, testnetConfig.gatewayURL);
}
````

The `InitAPI()` function receives a `CESSConfig` object that has the following shape:

```ts
interface CESSConfig {
  nodeURL: string;    // This is the RPC endpoint connecting to
  gatewayURL: string; // This is the DeOSS gateway URL. Please use the above provided one for testnet
  keyringOption: {
    type: string;     // By default it is SR25519 (ref: https://wiki.polkadot.network/docs/learn-cryptography#keypairs-and-signing)
    ss58Format: number; // Prefix for CESS. For CESS Testnet, it is `42`
  };
}
```

`testnetConfig` is a **CESSConfig** instance with configuration connecting to the CESS testnet.

After that, we can use the `api` and `keyring` returned to create **Space**, **Bucket**, and **File** services.

Each of these services requires both the `api` and `keyring` to be passed in as parameters.

Let's take a deeper look at using **Space** service.

## Using the Space Service

Let's look at the [function `checkNRentSpace()`](https://github.com/CESSProject/cess-course/blob/308ec7fe053e92c08e4c2d634579f84b359072ac/examples/js/src/index.ts#L57).

```ts
async function checkNRentSpace(space: Space, common: Common) {
  const initSpace = await space.userOwnedSpace(acctId);
  console.log("query userOwnedSpace:", initSpace.data);

  const blockHeight = await common.queryBlockHeight();
  console.log("current block height:", blockHeight);

  let spaceData = common.formatSpaceInfo(initSpace.data, blockHeight);
  console.log("initial user space:", spaceData);

  if (!spaceData.totalSpace) {
    const rsResult = await space.buySpace(mnemonic, RENT_SPACE);
    console.log(rsResult);
  }
}
```

You can query your used space with `space.userOwnedSpace()`. It returns an object similar to the following:

```ts
{
  totalSpace: 40802189312,
  usedSpace: 150994944,
  lockedSpace: 75497472,
  remainingSpace: 40600862720,
  start: 403800,
  deadline: 1397400,
  state: 'normal'
}
```

There is a utility class **Common** that formats the above data to be more readable with

```
common.formatSpaceInfo(initSpace.data, blockHeight)`
```

It returns the following object:

```ts
{
  totalSpace: 40802189312,
  usedSpace: 150994944,
  lockedSpace: 75497472,
  remainingSpace: 40600862720,
  start: 403800,
  deadline: 1397400,
  state: 'normal',
  totalSpaceGib: 38,
  totalSpaceStr: '38.00 GB',
  usedSpaceGib: 0.140625,
  usedSpaceStr: '144.00 MB',
  lockedSpaceGib: 0.0703125,
  lockedSpaceStr: '72.00 MB',
  remainingSpaceGib: 37.8125,
  remainingSpaceStr: '37.81 GB',
  deadlineTime: '2023-12-17 13:42:07',
  remainingDays: 52
}
```

You may notice a `blockHeight` is passed in as a parameter in `formatSpaceInfo()` to calculate the `deadlineTime`.

If this is the first time you rent spaces from CESS, use the API:

```
buySpace(mnemonic: string, gibCount: number): Promise<any>
```

Otherwise, you can use `expandSpace()` to expand the amount of capacity and `renewalSpace()` to extend the length of the space leased.

- `expansionSpace(mnemonicOrAccountId32: string, gibCount: number): Promise<any>`
- `renewalSpace(mnemonic: string, days: number): Promise<any>`

## Using the Bucket Service

We use the **Bucket** service to query information about buckets created by users (a concept similar to directories in our local hard drive), create buckets, and delete buckets.

Let's look at the code in [`checkNCreateBucket()`](https://github.com/CESSProject/cess-course/blob/308ec7fe053e92c08e4c2d634579f84b359072ac/examples/js/src/index.ts#L73).

```ts
async function checkNCreateBucket(bucket: Bucket) {
  let res = await bucket.queryBucketList(acctId);
  console.log("queryBucketList", res.data);

  res = await bucket.queryBucketInfo(acctId, BUCKET_NAME);
  console.log("queryBucketInfo", res);

  if (res.data) {
    console.log("deleting bucket...");
    // The bucket exists already, so let's delete it first
    await bucket.deleteBucket(mnemonic, acctId, BUCKET_NAME);
  }

  res = await bucket.createBucket(mnemonic, acctId, BUCKET_NAME);
  console.log("createBucket", res);

  res = await bucket.queryBucketList(acctId);
  console.log("queryBucketList", res.data);
}
```

`queryBucketList()` returns an array of objects similar to the following:

```ts
[
  {
    objectList: [
      '7b7eba922840cc2df17a5ee5ff9504ca56d857dc5938a10dd9e0ddaa1b7a0e3f',
      // hash values
    ],
    key: 'xhftBucket'
  },
  { objectList: [], key: 'test' },
  { objectList: [], key: 'test2' }
]
```

Each item is a bucket with the `key` as its bucket name. So this user has three buckets, named `xhftBucket`, `test`, and `test2`. `xhftBucket` bucket has one item inside, with `test` and `test2` being two empty buckets.

We can query more details about a particular bucket with `queryBucketInfo()`. It returns an object with the following shape.

```ts
{
  objectList: [],
  authority: [ 'cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB' ]
}
```

It contains information about what files inside the bucket and users who have write access to them. By default, only the creator of the bucket can write to it. But users can use **Authorize** service to grant this access to other users.

You can create a bucket with `createBucket()` call and delete a bucket with `deleteBucket()`. Both functions require you to pass in the mnemonic as you will send a signed transaction (requiring your private key) to the CESS blockchain.

## Using the File Service

Let's look at how to query our uploaded files and manage them by looking into [`uploadNDownloadFile()` function](https://github.com/CESSProject/cess-course/blob/308ec7fe053e92c08e4c2d634579f84b359072ac/examples/js/src/index.ts#L93).

```ts
async function uploadNDownloadFile(fileService: File) {
  let res = await fileService.queryFileListFull(acctId);
  console.log("queryFileListFull", res.data);
  const { fileHash } = res.data[0];

  const uploadFile = `${process.cwd()}/package.json`;
  console.log("uploadFile path:", uploadFile);
  res = await fileService.uploadFile(mnemonic, acctId, uploadFile, BUCKET_NAME);
  console.log("uploadFile", res);

  const downloadPath = `${process.cwd()}/download`;
  res = await fileService.downloadFile(fileHash, downloadPath);
  console.log("downloadFile", res);
}
```

We use `queryFileListFull()` to query the files that the user is accessible to. It returns an object similar to the following.

```ts
[
  {
    fileHash: '7b7eba922840cc2df17a5ee5ff9504ca56d857dc5938a10dd9e0ddaa1b7a0e3f',
    fileSize: 25165824,
    fileSizeStr: '29.00 B',
    fileName: 'hello.world',
    bucketName: 'xhftBucket',
    stat: 'Active'
  },
  {
    fileHash: 'b50b924f6292da622e6bb68407738becba1d19d6bffe18b7d5d7b37f819c312d',
    fileSize: 25165824,
    fileSizeStr: '24.00 B',
    fileName: 'xhft_test20231009.txt',
    bucketName: 'xhftBucket',
    stat: 'Active'
  },
  // ...
]
```

It contains the file name, file size, belonging bucket, and the file hash.

To upload a file to the CESS cloud, use `uploadFile()`. It takes the upload file path and bucket name as parameters in addition to the account mnemonic.

Once the file is uploaded, you can query the file on the cloud to get its hash and use the hash value to download the file back with `downloadFile()`.

# API

**Space**

This service is for managing user space. You can query, renew, and expand spaces used in CESS.

- `userOwnedSpace(accountId32: string): Promise<APIReturnedData>`
- `buySpace(mnemonic: string, gibCount: number): Promise<any>`
- `expansionSpace(mnemonicOrAccountId32: string, gibCount: number): Promise<any>`
- `renewalSpace(mnemonic: string, days: number): Promise<any>`

**Bucket**

This service is for managing user buckets, a concept similar to directory in a local hard drive.

- `queryBucketNames(accountId32: string): Promise<APIReturnedData>`
- `queryBucketList(accountId32: string): Promise<APIReturnedData>`
- `queryBucketInfo(accountId32: string, name: string): Promise<APIReturnedData>`
- `createBucket(mnemonic: string, accountId32: string, name: string): Promise<any>`
- `deleteBucket(mnemonic: string, accountId32: string, name: string): Promise<any>`

**File**

This service is for managing user files in the CESS cloud.

- `queryFileListFull(accountId32: string): Promise<APIReturnedData>`
- `queryFileList(accountId32: string): Promise<APIReturnedData>`
- `queryFileMetadata(fileHash: string): Promise<APIReturnedData>`
- `uploadFile(mnemonic: string, accountId32: string, filePath: string, bucketName: string): Promise<any>`
- `downloadFile(fileHash: string, savePath: string): Promise<any>`
- `deleteFile(mnemonic: string, accountId32: string, fileHashArray: string[]): Promise<any>`

**Authorize**

This service is for managing delegated rights to other users.

- `authorityList(accountId32: string): Promise<APIReturnedData>`
- `authorize(mnemonic: string, operator: string): Promise<any>`
- `cancelAuthorize(mnemonic: string, operator: string): Promise<any>`

# Conclusion

With **cess-js-sdk** APIs, you can incorporate CESS cloud into your products. If you have any feedback about this tutorial, feel free to [contact us](../../introduction/contact.md).
