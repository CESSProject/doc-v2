This is the interface to query the completed challenge snapshots of the miner.

```golang
// QueryCompleteMinerSnapShot query the completed challenge snapshots of miners
//   - puk: account id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []MinerCompleteInfo: list of completed challenge snapshots
//   - error: error message
func (c *ChainClient) QueryCompleteMinerSnapShot(puk []byte, block int32) ([]MinerCompleteInfo, error)
```

For the type definition, please refer to [MinerCompleteInfo](../chain_type.md#MinerCompleteInfo)

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

    fmt.Println(sdk.QueryCompleteMinerSnapShot(0, -1))
}
```