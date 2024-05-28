This interface is used to query all files uploaded by the user.

```golang
// QueryAllUserFiles query user's all files
//   - accountID: user account
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []string: all file identification
//   - error: error message
func (c *ChainClient) QueryAllUserFiles(accountID []byte, block int32) ([]string, error)
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

	fmt.Println(sdk.QueryAllUserFiles(account_id, -1))
}
```