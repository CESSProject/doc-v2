This is the interface to query the points of each verifier in an era, and calculate the reward of each verifier according to the points ratio, this interface must wait until the end of the era to call.

```golang
// QueryErasRewardPoints query the rewards of consensus nodes in each era
//   - era: era id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - StakingEraRewardPoints: the rewards of consensus nodes
//   - error: error message
func (c *ChainClient) QueryErasRewardPoints(era uint32, block int32) (StakingEraRewardPoints, error)
```

For the type definition, please refer to [StakingEraRewardPoints](../chain_type.md#StakingEraRewardPoints)

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

    fmt.Println(sdk.QueryErasRewardPoints(0, -1))
}
```