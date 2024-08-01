This is the interface to query the current block synchronisation information.

```golang
// SystemSyncState query system sync state
//
// Return:
//   - SysSyncState: system sync state
//   - error: error message
func (c *ChainClient) SystemSyncState() (SysSyncState, error)
```

For the type definition, please refer to [SysSyncState](../chain_type.md#SysSyncState)

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

	fmt.Println(sdk.SystemSyncState())
}
```