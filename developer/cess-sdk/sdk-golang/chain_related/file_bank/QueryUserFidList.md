This interface is used to query all fids uploaded by the user.

```golang
// QueryUserFidList query user's all fids
//   - accountID: user account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []string: all fids
//   - error: error message
func (c *ChainClient) QueryUserFidList(accountID []byte, block int32) ([]string, error)
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

    fmt.Println(sdk.QueryUserFidList(account_id, -1))
}
```