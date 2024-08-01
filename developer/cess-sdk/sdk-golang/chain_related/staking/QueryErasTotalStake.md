This is the interface to query the total number of staking in an era.

```golang
// QueryErasTotalStake query the total number of staking for each era
//   - era: era id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: the total number of staking
//   - error: error message
func (c *ChainClient) QueryErasTotalStake(era uint32, block int32) (string, error)
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

    fmt.Println(sdk.QueryErasTotalStake(0, -1))
}
```