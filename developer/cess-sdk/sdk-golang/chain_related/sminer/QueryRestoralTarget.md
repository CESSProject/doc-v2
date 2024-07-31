This is the interface for querying file recovery information for exited storage miners.

```golang
// QueryRestoralTarget query the data recovery information of exited storage miner
//   - accountID: storage miner account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - RestoralTargetInfo: the data recovery information
//   - error: error message
func (c *ChainClient) QueryRestoralTarget(accountID []byte, block int32) (RestoralTargetInfo, error)
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

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.QueryRestoralTarget(account_id, -1))
}
```