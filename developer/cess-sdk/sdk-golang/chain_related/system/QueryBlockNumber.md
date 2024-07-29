This is the interface to query the block number based on the block hash.

```golang
// QueryBlockNumber query the block number based on the block hash
//   - blockhash: hex-encoded block hash, if empty query the latest block number
//
// Return:
//   - uint32: block number
//   - error: error message
func (c *ChainClient) QueryBlockNumber(blockhash string) (uint32, error)
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

    fmt.Println(sdk.QueryBlockNumber("0x31d396f63cff05a52784a6...b80433d7dd9c4a98509a"))
}
```