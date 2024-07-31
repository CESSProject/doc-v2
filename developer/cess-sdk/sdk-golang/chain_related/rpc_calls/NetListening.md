This is the interface to check if the current RPC connection is normal.

```golang
// NetListening query net listenning
//
// Return:
//   - bool: net listenning
//   - error: error message
func (c *ChainClient) NetListening() (bool, error)
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

	fmt.Println(sdk.NetListening())
}
```