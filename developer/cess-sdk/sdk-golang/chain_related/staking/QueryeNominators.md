This is the interface for querying the account information nominated by the nominator.

```golang
// QueryeNominators query the nominator info
//   - accountId: account id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - StakingNominations: nominator info
//   - error: error message
func (c *ChainClient) QueryeNominators(accountId []byte, block int32) (StakingNominations, error)
```
For the type definition, please refer to [StakingNominations](../chain_type.md#StakingNominations)
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

    fmt.Println(sdk.QueryeNominators(account_id, -1))
}
```