This is the interface for querying the chain version number.

```golang
// SystemVersion query system version
//
// Return:
//   - string: system version
//   - error: error message
func (c *ChainClient) SystemVersion() (string, error)
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

	fmt.Println(sdk.SystemVersion())
}
```