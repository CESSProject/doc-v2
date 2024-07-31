QueryOss query oss (gateway) information.

```golang
// QueryOss query oss info
//   - accountID: oss's account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - OssInfo: oss info
//   - error: error message
func (c *ChainClient) QueryOss(accountID []byte, block int32) (OssInfo, error)
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

    fmt.Println(sdk.QueryOss(account_id, -1))
}
```