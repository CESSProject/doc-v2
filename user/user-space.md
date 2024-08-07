# Background

CESS testnet has been released. If users want to upload files, they need to purchase space and an authorized gateway first. This guide is for developers who want to interact directly with the chain through a blockchain browser to purchase territory, expand territory, and other operations.

## What is territory

To allow users to use the storage space of the CESS network more flexibly, we have decided to give the storage space the characteristics of NFTs, making it unique and tradable. This is intended to activate the network. We have thus introduced the concept of "CESS territory."

"Territory" replaces the previous concept of storage space, converting user-held space into user-held territory. The files uploaded by users will be uploaded to the territory they own.

Each user can have an unlimited number of "territories." Users can name each of their territories. At the same time, each territory has a unique token value for identification. Users can trade or transfer their territories using the territory token value. Since this is an expansion of the previous version of storage space, each territory will also have an expiration time. Users can renew and expand their existing territories through renewal or expansion.

# Guides

The guide will explain how to purchase territory, expand territory, renew territory, and some of the details.

## Buy Territory

1. After connecting to rpc node, the browser loads the blockchain information. We select Developer - Extrinsics as shown in the image below.

![Enter The Transaction Page](../../assets/developer/guides/space-operation/pic1.png)

2. Select the `StorageHandler` module and select the `mintTerritory`.

![mint territory](../../assets/developer/guides/territory-operation/buy_territory.png)

3. Click `Sign and Submit` to confirm the signature, Then wait for the browser to call the wallet to sign. After entering your customized password, click `Sign the transaction` to sign the transaction.

![Signature Transaction](../../assets/developer/guides/space-operation/pic5.png)

4. Wait for the transaction to be packaged and then broadcast the confirmation. If you see the content shown in Figure 2 below, it means that the transaction you submitted has been successfully executed.

        It should be noted that if your account has already purchased space, calling this transaction will fail. If you want to expand or renew the lease, please call the corresponding transaction.

![Broadcast](../../assets/developer/guides/space-operation/pic6.png)

![Success](../../assets/developer/guides/space-operation/pic7.png)

## Renewal Territory

1. Select `Developer -> Extrinsics -> StorageHandler` module, then select the `renewalTerritory`, after filling in the parameters `territoryName` and  `days` correctly, submit the transaction.

       `territoryName` is the name of the territory you have purchased.
       `days` is the increased validity period days.

![Renewal Territory](../../assets/developer/guides/territory-operation/renewal_territory.jpg)

2. The subsequent operations are the same as purchasing space.

## Expand Territory

1. Select `Developer -> Extrinsics -> StorageHandler` module, then select the `expandingTerritory`, after filling in the parameters `territoryName` and  `gibCount` correctly, submit the transaction.

       `territoryName` is the name of the territory you have purchased.
       `gibCount` is the size of the expansion, in GiB.

![Expand Territory](../../assets/developer/guides/territory-operation/expanding_territory.jpg)

2. The subsequent operations are the same as purchasing space.

## Query Territory

1. Select `Developer -> Chain state -> StorageHandler` module, then select the `territory`, after filling in the parameters `AccountID32` and  `Bytes` correctly, click "+" to query. View the returned results.

       `AccountID32` is the account address where you purchased the territory.
       `Bytes` is the name of the territory.

![Query Territory](../../assets/developer/guides/territory-operation/query_territory.jpg)

`token`: is the unique identifier of the territory.

`totalSpace`: Indicates the total space currently held by the user, in bytes.

`usedSpace`: Indicates the space currently used by the user, in bytes.

`lockedSpace`: Indicates the space used by the file currently being uploaded by the user, in bytes.

`remainingSpace`: Indicates the remaining space available to the user, in bytes.  

`start`: Block height when space is first purchased.

`deadline`: Block height at expiration.

`state`: The status of the space currently held by the user.