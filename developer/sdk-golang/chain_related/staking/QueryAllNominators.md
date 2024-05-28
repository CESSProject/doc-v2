This is the interface to query all nominations.

```golang
// QueryAllNominators query all nominators info
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []StakingNominations: all nominators info
//   - error: error message
func (c *ChainClient) QueryAllNominators(block int32) ([]StakingNominations, error)
```

For the type definition, please refer to [StakingNominations](../chain_type.md#StakingNominations)

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

	fmt.Println(sdk.QueryAllNominators(-1))
}
```