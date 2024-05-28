This is the interface for the storage miner to report that the fragment transfer is complete, telling the chain that it has been stored.

```golang
// TransferReport is used by miners to report that a file has been transferred
//   - index: index of the file fragment
//   - fid: file identification
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - for storage miner use only
func (c *ChainClient) TransferReport(index uint8, fid string) (string, error)
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

	fmt.Println(sdk.TransferReport(0, "b984d0de1428d0011...a26d41f3f7abaa5b6c450"))
}
```