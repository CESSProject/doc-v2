This is the interface to query the account of the validator being validated.

```golang
// QueryValidators query validators account (waiting nodes not included)
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []types.AccountID: validators account
//   - error: error message
func (c *ChainClient) QueryValidators(block int32) ([]types.AccountID, error)
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

    fmt.Println(sdk.QueryValidators(-1))
}
```