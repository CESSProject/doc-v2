This is the interface used to delete files.

```golang
// DeleteFile delete a bucket for owner
//   - owner: file owner account
//   - fid: file identification
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - if you are not the owner, the owner account must be authorised to you
func (c *ChainClient) DeleteFile(owner []byte, fid string) (string, error)
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

	fmt.Println(sdk.DeleteFile(sdk.GetSignatureAccPulickey(), "b984d0de1428d0011...a26d41f3f7abaa5b6c450"))
}
```