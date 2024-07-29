This is the interface for querying the rewards of an era.

```golang
// QueryEraReward query the rewards in era
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: rewards in era
//   - error: error message
func (c *ChainClient) QueryEraReward(block int32) (string, error)
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

    fmt.Println(sdk.QueryEraReward(-1))
}
```