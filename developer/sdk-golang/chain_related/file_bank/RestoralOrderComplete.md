This is the interface for the storage miner to report that the fragment recovery is complete, calling this interface indicates that you have completed the recovery.

```golang
// RestoralOrderComplete submits the restored completed order
//   - fragmentHash: fragment hash
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - for storage miner use only
func (c *ChainClient) RestoralOrderComplete(fragmentHash string) (string, error)
```

Example code:
```golang
package main

import (
	"context"
	"fmt"
	"time"

	sdkgo "github.com/CESSProject/cess-go-sdk"
)

// Substrate well-known mnemonic:
//
//   - https://github.com/substrate-developer-hub/substrate-developer-hub.github.io/issues/613
//   - cXgaee2N8E77JJv9gdsGAckv1Qsf3hqWYf7NL4q6ZuQzuAUtB
var MY_MNEMONIC = "bottom drive obey lake curtain smoke basket hold race lonely fit walk"

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
		sdkgo.Mnemonic(MY_MNEMONIC),
		sdkgo.TransactionTimeout(time.Second*10),
	)
	if err != nil {
		panic(err)
	}
	defer sdk.Close()

	fmt.Println(sdk.RestoralOrderComplete("50c54b1da4029f201465...7c8b378b6daecc0b674"))
}
```