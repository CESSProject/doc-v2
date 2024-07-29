QueryChallengeSnapShot query challenge snapshot data for storage miner.


```golang
// QueryChallengeSnapShot query challenge snapshot data
//   - accountID: signature account of the storage miner
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - bool: is there any challenge snapshot data
//   - ChallengeInfo: challenge snapshot data
//   - error: error message
func (c *ChainClient) QueryChallengeSnapShot(accountID []byte, block int32) (bool, ChallengeInfo, error)
```
The return type is detailed in [ChallengeInfo](../chain_type.md#ChallengeInfo).

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
    fmt.Println(sdk.QueryChallengeSnapShot(account_id, -1))
}
```