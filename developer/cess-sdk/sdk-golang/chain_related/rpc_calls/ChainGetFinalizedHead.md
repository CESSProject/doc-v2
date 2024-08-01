This is the interface for querying the hash of the most recently determined block.

```golang
// ChainGetFinalizedHead get finalized block hash
//
// Return:
//   - types.Hash: block hash
//   - error: error message
func (c *ChainClient) ChainGetFinalizedHead() (types.Hash, error)
```

Example code:
```golang
package main

import (
	"context"
	"fmt"

	sdkgo "github.com/CESSProject/cess-go-sdk"
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

	fmt.Println(sdk.ChainGetFinalizedHead())
}
```