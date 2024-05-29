This is the interface for querying storage miner rewards.

```golang
// QueryRewardMap query all reward information for storage miner
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - MinerReward: all reward information
//   - error: error message
func (c *ChainClient) QueryRewardMap(accountID []byte, block int32) (MinerReward, error)
```

For the type definition, please refer to [MinerReward](../chain_type.md#MinerReward)

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

    account_id, err := utils.ParsingPublickey("cXfyomKDABfehLkvARFE854wgDJFMbsxwAJEHezRb6mfcAi2y")
    if err != nil {
        panic(err)
    }

    fmt.Println(sdk.QueryRewardMap(account_id, -1))
}
```