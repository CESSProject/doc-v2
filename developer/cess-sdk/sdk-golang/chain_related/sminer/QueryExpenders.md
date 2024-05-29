This is the interface for querying idle data specifications.

```golang
// QueryExpenders query expenders (idle data specification)
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - ExpendersInfo: idle data specification
//   - error: error message
func (c *ChainClient) QueryExpenders(block int32) (ExpendersInfo, error)
```
For the type definition, please refer to [ExpendersInfo](../chain_type.md#ExpendersInfo)

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

    fmt.Println(sdk.QueryExpenders(-1))
}
```