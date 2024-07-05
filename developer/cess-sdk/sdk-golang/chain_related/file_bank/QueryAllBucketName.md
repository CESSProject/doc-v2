This interface is used to query the names of all storage buckets created by the user.

```golang
// QueryAllBucketName query user's all bucket names
//   - accountID: user account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []string: all bucket names
//   - error: error message
func (c *ChainClient) QueryAllBucketName(accountID []byte, block int32) ([]string, error)
```

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

    fmt.Println(sdk.QueryAllBucketName(account_id, -1))
}
```