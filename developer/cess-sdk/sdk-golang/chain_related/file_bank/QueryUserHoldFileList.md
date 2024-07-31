This interface is used to query all files uploaded by the user.

```golang
// QueryUserHoldFileList query user's all files
//   - accountID: user account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []UserFileSliceInfo: file list
//   - error: error message
func (c *ChainClient) QueryUserHoldFileList(accountID []byte, block int32) ([]UserFileSliceInfo, error)
```

The return type is detailed in [UserFileSliceInfo](../chain_type.md#UserFileSliceInfo).

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

    fmt.Println(sdk.QueryUserHoldFileList(account_id, -1))
}
```