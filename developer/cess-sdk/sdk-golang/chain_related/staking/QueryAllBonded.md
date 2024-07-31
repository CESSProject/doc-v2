This is the interface for querying all accounts that are bound to participate in consensus or voting.

```golang
// QueryAllBonded query all consensus and nominators accounts
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []types.AccountID: all consensus and nominators accounts
//   - error: error message
func (c *ChainClient) QueryAllBonded(block int32) ([]types.AccountID, error)
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

    fmt.Println(sdk.QueryAllBonded(-1))
}
```