This is the interface for querying chain names.

```golang
// SystemProperties query system properties
//
// Return:
//   - string: system chain
//   - error: error message
func (c *ChainClient) SystemChain() (string, error)
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

	fmt.Println(sdk.SystemChain())
}
```