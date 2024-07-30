This is the interface to query the number of consensus nodes being verified.

```golang
// QueryValidatorsCount query validator number (waiting nodes not included)
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint32: validator number
//   - error: error message
func (c *ChainClient) QueryValidatorsCount(block int32) (uint32, error)
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

    fmt.Println(sdk.QueryValidatorsCount(-1))
}
```