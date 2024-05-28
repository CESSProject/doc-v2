This is the interface to query the size of idle space that can be deleted by storage miners, only after storing a service file.

```golang
// QueryPendingReplacements query the size of the storage miner's replaceable idle data
//   - accountID: storage miner account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - types.U128: the size of replaceable idle data
//   - error: error message
func (c *ChainClient) QueryPendingReplacements(accountID []byte, block int32) (types.U128, error)
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
    "wss://testnet-rpc0.cess.cloud/ws/",
    "wss://testnet-rpc1.cess.cloud/ws/",
    "wss://testnet-rpc2.cess.cloud/ws/",
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

    account_id, err := utils.ParsingPublickey("cXfyomKDABfehLkvARFE854wgDJFMbsxwAJEHezRb6mfcAi2y")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.QueryPendingReplacements(account_id, -1))
}
```