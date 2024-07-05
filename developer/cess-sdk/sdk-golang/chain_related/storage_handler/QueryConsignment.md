This is the interface to query the consignment info.

```golang
// QueryConsignment query consignment info
//   - token: territory key
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - ConsignmentInfo: consignment info
//   - error: error message
func (c *ChainClient) QueryConsignment(token types.H256, block int32) (ConsignmentInfo, error) 
```

The return type is detailed in [ConsignmentInfo](../chain_type.md#ConsignmentInfo).

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

    territoryinfo, err := sdk.QueryTerritory(account_id, "territory_name", -1)
	if err != nil {
		panic(err)
	}

    fmt.Println(sdk.QueryConsignment(territoryinfo.Token, -1))
}
```