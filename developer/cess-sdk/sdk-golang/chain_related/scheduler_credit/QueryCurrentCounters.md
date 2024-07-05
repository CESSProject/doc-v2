This is the interface to query the verifier's reputation score.

```golang
// QueryCurrentCounters query the validator's credit score
//   - accountId: validator's account id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - SchedulerCounterEntry: validator's credit score
//   - error: error message
func (c *ChainClient) QueryCurrentCounters(accountId []byte, block int32) (SchedulerCounterEntry, error)
```

For the type definition, please refer to [SchedulerCounterEntry](../chain_type.md#SchedulerCounterEntry)

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

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.QueryCurrentCounters(account_id, -1))
}
```