This interface is used to query file meta information.

```golang
// QueryFile query file metadata
//   - fid: file identification
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - FileMetadata: file metadata
//   - error: error message
func (c *ChainClient) QueryFile(fid string, block int32) (FileMetadata, error)
```
The return type is detailed in [FileMetadata](../chain_type.md#StorageOrder).

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

	fmt.Println(sdk.QueryFile("b984d0de1428d0011...a26d41f3f7abaa5b6c450", -1))
}
```