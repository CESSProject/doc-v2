This is the interface for querying file recovery information for all exited storage miners.

```golang
// QueryAllRestoralTarget query the data recovery information of all exited storage miner
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []RestoralTargetInfo: all the data recovery information
//   - error: error message
func (c *ChainClient) QueryAllRestoralTarget(block int32) ([]RestoralTargetInfo, error)
```
For the type definition, please refer to [RestoralTargetInfo](../chain_type.md#RestoralTargetInfo)

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

    fmt.Println(sdk.QueryAllRestoralTarget(-1))
}
```