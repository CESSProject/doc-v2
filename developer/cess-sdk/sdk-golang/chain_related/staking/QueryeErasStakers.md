This is the interface for querying information about verifier staking in an era.

```golang
// QueryeErasStakers query the staking exposure
//   - era: era id
//   - accountId: account id
//
// Return:
//   - StakingExposure: staking exposure
//   - error: error message
func (c *ChainClient) QueryeErasStakers(era uint32, accountId []byte) (StakingExposure, error)
```
For the type definition, please refer to [StakingExposure](../chain_type.md#StakingExposure)
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

    fmt.Println(sdk.QueryeErasStakers(0, account_id))
}
```