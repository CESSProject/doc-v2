This is the interface to query all rewards in an era.

```golang
// QueryEraValidatorReward query the total rewards for each era
//   - era: era id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: total rewards
//   - error: error message
func (c *ChainClient) QueryEraValidatorReward(era uint32, block int32) (string, error)
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

    fmt.Println(sdk.QueryEraValidatorReward(0, -1))
}
```