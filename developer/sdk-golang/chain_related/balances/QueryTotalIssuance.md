QueryTotalIssuance query the total amount of token issuance.


```golang
// QueryTotalIssuance query the total amount of token issuance
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: the total amount of token issuance
//   - error: error message
func (c *ChainClient) QueryTotalIssuance(block int32) (string, error)
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
	"wss://testnet-rpc0.cess.cloud/ws/",
	"wss://testnet-rpc1.cess.cloud/ws/",
	"wss://testnet-rpc2.cess.cloud/ws/",
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

	fmt.Println(sdk.QueryTotalIssuance(-1))
}
```