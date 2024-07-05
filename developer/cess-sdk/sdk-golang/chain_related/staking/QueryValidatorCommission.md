This is the interface for querying the commission ratio of validators.

```golang
// QueryValidatorCommission query validator commission
//   - accountID: validator account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint8: validator commission
//   - error: error message
func (c *ChainClient) QueryValidatorCommission(accountID []byte, block int32) (uint8, error)
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

    fmt.Println(sdk.QueryValidatorCommission(account_id, -1))
}
```