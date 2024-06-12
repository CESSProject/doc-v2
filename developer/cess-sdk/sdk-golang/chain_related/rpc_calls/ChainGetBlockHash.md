This is the interface to query the block hash based on the block number.

```golang
// ChainGetBlockHash get block hash by block number
//
// Return:
//   - types.Hash: block hash
//   - error: error message
func (c *ChainClient) ChainGetBlockHash(block uint32) (types.Hash, error)
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

	fmt.Println(sdk.ChainGetBlockHash(0))
}
```