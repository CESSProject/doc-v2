This is the interface for querying all account information.

```golang
// QueryAllAccountInfo query all account information
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []types.AccountInfo: all account info
//   - error: error message
func (c *ChainClient) QueryAllAccountInfo(block int32) ([]types.AccountInfo, error)
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
    "wss://testnet-rpc.cess.network/ws/",
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

    fmt.Println(sdk.QueryAllAccountInfo(1000))
}
```