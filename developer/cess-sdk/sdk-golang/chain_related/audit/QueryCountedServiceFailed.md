QueryCountedServiceFailed query the number of failed service data challenge.


```golang
// QueryCountedServiceFailed query the number of failed service data challenge
//   - accountID: signature account of the storage miner
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint32: Is there a value
//   - error: error message
func (c *ChainClient) QueryCountedServiceFailed(accountID []byte, block int32) (uint32, error)
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

    account_id, err := utils.ParsingPublickey("cX...")
    if err != nil {
        panic(err)
    }
    fmt.Println(sdk.QueryCountedServiceFailed(account_id, -1))
}
```