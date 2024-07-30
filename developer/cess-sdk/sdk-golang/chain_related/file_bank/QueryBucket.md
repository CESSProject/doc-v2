This interface is used to query bucket information created by sstorage users.

```golang
// QueryBucket query user's bucket information
//   - accountID: user account
//   - bucketName: bucket name
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - BucketInfo: bucket info
//   - error: error message
func (c *ChainClient) QueryBucket(accountID []byte, bucketName string, block int32) (BucketInfo, error)
```
The return type is detailed in [BucketInfo](../chain_type.md#BucketInfo).

Example code:
```golang
package main

import (
    "context"
    "fmt"
    "time"

    sdkgo "github.com/CESSProject/cess-go-sdk"
    "github.com/CESSProject/cess-go-sdk/utils"
)

var RPC_ADDRS = []string{
    //testnet
    "wss://testnet-rpc.cess.cloud/ws/",
}

func main() {
    sdk, err := sdkgo.New(
        context.Background(),
        sdkgo.ConnectRpcAddrs(RPC_ADDRS),
    )
    if err != nil {
        panic(err)
    }
    defer sdk.Close()

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.QueryBucket(account_id, -1))
}
```