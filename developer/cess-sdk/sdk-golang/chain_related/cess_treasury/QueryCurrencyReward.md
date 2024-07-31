This is the interface to query the current reward.

```golang
// QueryCurrencyReward query the currency rewards
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: currency rewards
//   - error: error message
func (c *ChainClient) QueryCurrencyReward(block int32) (string, error)
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

    fmt.Println(sdk.QueryCurrencyReward(-1))
}
```