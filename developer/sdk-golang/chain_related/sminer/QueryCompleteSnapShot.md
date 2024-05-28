This is the interface to query the total number of storage miners and the total power in an era.

```golang
// QueryCompleteSnapShot query the number of storage miners and storage miner power in each era
//   - era: era id
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint32: the number of storage miners in current era
//   - uint64: all storage miners power in current era
//   - error: error message
func (c *ChainClient) QueryCompleteSnapShot(era uint32, block int32) (uint32, uint64, error)
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

    account_id, err := utils.ParsingPublickey("cXfyomKDABfehLkvARFE854wgDJFMbsxwAJEHezRb6mfcAi2y")
	if err != nil {
		panic(err)
	}

	fmt.Println(sdk.QueryCompleteSnapShot(0, -1))
}
```