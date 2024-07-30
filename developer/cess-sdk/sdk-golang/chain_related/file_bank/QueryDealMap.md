This interface is used to query file storage orders.

```golang
// QueryDealMap query file storage order
//   - fid: file identification
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - StorageOrder: file storage order
//   - error: error message
func (c *ChainClient) QueryDealMap(fid string, block int32) (StorageOrder, error)
```
The return type is detailed in [StorageOrder](../chain_type.md#StorageOrder).

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

    fmt.Println(sdk.QueryDealMap("b984d0de1428d0011...a26d41f3f7abaa5b6c450", -1))
}
```

> Note: The file upload process must first create a file storage order, wait for all fragments to be stored in the storage miner after the order is completed, and then the file information is moved to the meta information to display.
