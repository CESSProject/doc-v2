This is the interface for querying the era where it is located based on the block number.

```golang
// QueryCurrentEra query the current era id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint32: era id
//   - error: error message
func (c *ChainClient) QueryCurrentEra(block int32) (uint32, error)
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

    fmt.Println(sdk.QueryCurrentEra(0))
}
```