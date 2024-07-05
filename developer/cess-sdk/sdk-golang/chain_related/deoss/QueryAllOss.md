QueryAllOss query all oss (gateway) information.

```golang
// QueryAllOss query all oss info
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []OssInfo: all oss info
//   - error: error message
func (c *ChainClient) QueryAllOss(block int32) ([]OssInfo, error)
```
The return type is detailed in [OssInfo](../chain_type.md#OssInfo).

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

    fmt.Println(sdk.QueryAllOss(-1))
}
```