This is the interface for querying block information based on block hash.

```golang
// ChainGetBlock get SignedBlock info by block hash
//
// Return:
//   - types.SignedBlock: SignedBlock info
//   - error: error message
func (c *ChainClient) ChainGetBlock(hash types.Hash) (types.SignedBlock, error)
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

	blockhash, err := sdk.ChainGetBlockHash(0)
	if err != nil {
		panic(err)
	}

	fmt.Println(sdk.ChainGetBlock(blockhash))
}
```