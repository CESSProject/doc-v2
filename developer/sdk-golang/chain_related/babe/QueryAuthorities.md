QueryAuthorities query the RRSC app public key for consensus nodes.

```golang
// QueryAuthorities query consensus rrsc public
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []ConsensusRrscAppPublic: all consensus rrsc public
//   - error: error message
func (c *ChainClient) QueryAuthorities(block int32) ([]ConsensusRrscAppPublic, error)
```

The return type is detailed in [ConsensusRrscAppPublic](../chain_type.md#ConsensusRrscAppPublic).

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

	fmt.Println(sdk.QueryAuthorities(-1))
}
```