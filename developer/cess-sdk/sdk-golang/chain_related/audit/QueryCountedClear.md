QueryCountedClear query the number of times to clear the challenge failure count.


```golang
// QueryCounterdClear query the number of times to clear the challenge failure count
//   - accountID: signature account of the storage miner
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint8: cleanup count
//   - error: error message
func (c *ChainClient) QueryCountedClear(accountID []byte, block int32) (uint8, error)
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

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }
    fmt.Println(sdk.QueryCountedClear(account_id, -1))
}
```