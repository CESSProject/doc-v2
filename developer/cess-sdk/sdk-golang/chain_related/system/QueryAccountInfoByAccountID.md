This is the interface for querying account information.

```golang
// QueryAccountInfoByAccountID query account info
//   - accountID: account id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - types.AccountInfo: account info
//   - error: error message
func (c *ChainClient) QueryAccountInfoByAccountID(accountID []byte, block int32) (types.AccountInfo, error)
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

    fmt.Println(sdk.QueryAccountInfoByAccountID(account_id, -1))
}
```