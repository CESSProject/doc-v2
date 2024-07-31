This interface is used to query the file recovery order, the storage miner can store more files by recovering the files in this order.

```golang
// QueryRestoralOrder query file restoral order
//   - fragmentHash: fragment hash
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - RestoralOrderInfo: restoral order info
//   - error: error message
func (c *ChainClient) QueryRestoralOrder(fragmentHash string, block int32) (RestoralOrderInfo, error)
```
The return type is detailed in [RestoralOrderInfo](../chain_type.md#RestoralOrderInfo).

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

    fmt.Println(sdk.QueryRestoralOrder("b984d0de1428d0011...a26d41f3f7abaa5b6c450", -1))
}
```