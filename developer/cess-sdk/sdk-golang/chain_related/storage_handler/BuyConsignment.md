This is the interface purchase territories for consignment.

```golang
// BuyConsignment purchase territories for consignment
//   - token: territory key
//   - territory_name: renamed territory name
//
// Return:
//   - string: block hash
//   - error: error message
func (c *ChainClient) BuyConsignment(token types.H256, territory_name string) (string, error)
```

Example code:
```golang
package main

import (
    "context"
    "fmt"
    "time"

    sdkgo "github.com/CESSProject/cess-go-sdk"
)

// Substrate well-known mnemonic:
//
//   - https://github.com/substrate-developer-hub/substrate-developer-hub.github.io/issues/613
//   - cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB
var MY_MNEMONIC = "bottom drive obey lake curtain smoke basket hold race lonely fit walk"

var RPC_ADDRS = []string{
    //testnet
    "wss://testnet-rpc.cess.network/ws/",
}

func main() {
    sdk, err := sdkgo.New(
        context.Background(),
        sdkgo.ConnectRpcAddrs(RPC_ADDRS),
        sdkgo.Mnemonic(MY_MNEMONIC),
        sdkgo.TransactionTimeout(time.Second*10),
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

    fmt.Println(sdk.BuyConsignment(territoryinfo.token, "territory_name"))
}
```