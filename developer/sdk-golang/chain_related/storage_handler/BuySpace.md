This is the interface for purchasing storage space.

```golang
// BuySpace purchase space for current account
//   - count: size of space purchased in gib
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - if you have already purchased space and you are unable to purchase it again,
//     you have the option to expand your space.
func (c *ChainClient) BuySpace(count uint32) (string, error)
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

    // purchase 100GiB space
    fmt.Println(sdk.BuySpace(100))
}
```
> Note: The default space validity period is 432000 blocks (approximately 1 month)