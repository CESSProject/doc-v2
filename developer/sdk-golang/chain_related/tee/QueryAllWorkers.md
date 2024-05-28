This is the interface to query all tee worker information.

```golang
// QueryAllWorkers query all tee work info
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []WorkerInfo: all tee worker info
//   - error: error message
func (c *ChainClient) QueryAllWorkers(block int32) ([]WorkerInfo, error)
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

    fmt.Println(sdk.QueryAllWorkers(-1))
}
```