This is the interface to query the territory info.

```golang
// QueryTerritory query territory info
//   - accountId: account id
//   - name: territory name
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - TerritoryInfo: territory info
//   - error: error message
func (c *ChainClient) QueryTerritory(accountId []byte, name string, block int32) (TerritoryInfo, error)
```

The return type is detailed in [TerritoryInfo](../chain_type.md#TerritoryInfo).

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

    fmt.Println(sdk.QueryTerritory(account_id, "territory_name", -1))
}
```