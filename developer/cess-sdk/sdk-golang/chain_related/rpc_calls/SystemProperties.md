This is the interface for querying system Properties information.

```golang
// SystemProperties query system properties
//
// Return:
//   - SysProperties: system properties
//   - error: error message
func (c *ChainClient) SystemProperties() (SysProperties, error)
```

For the type definition, please refer to [SysProperties](../chain_type.md#SysProperties)

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
	"wss://testnet-rpc.cess.cloud/ws/",
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

	fmt.Println(sdk.SystemProperties())
}
```