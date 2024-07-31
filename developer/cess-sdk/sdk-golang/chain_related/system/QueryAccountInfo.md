This is the interface for querying account information.

```golang
// QueryAccountInfo query account info
//   - account: account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - types.AccountInfo: account info
//   - error: error message
func (c *ChainClient) QueryAccountInfo(account string, block int32) (types.AccountInfo, error)
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

    fmt.Println(sdk.QueryAccountInfo("cXjeCHQW3totBGhQXdAUAqjCNqk1NhiR3UK37czSeUak2pqGV", -1))
}
```