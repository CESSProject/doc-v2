This interface is used to query the file recovery order, the storage miner can store more files by recovering the files in this order.

```golang
// QueryAllRestoralOrder query all file restoral order
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []RestoralOrderInfo: all restoral order info
//   - error: error message
func (c *ChainClient) QueryAllRestoralOrder(block int32) ([]RestoralOrderInfo, error)
```
The return type is detailed in [RestoralOrderInfo](../chain_type.md#RestoralOrderInfo).

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

	fmt.Println(sdk.QueryAllRestoralOrder(-1))
}
```