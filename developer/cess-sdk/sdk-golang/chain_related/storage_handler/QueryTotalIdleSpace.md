This is the interface to query the total idle space size.

```golang
// QueryTotalIdleSpace query the size of all idle space
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint64: the size of all idle space
//   - error: error message
func (c *ChainClient) QueryTotalIdleSpace(block int32) (uint64, error)
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

    fmt.Println(sdk.QueryTotalIdleSpace(-1))
}
```