Before using SDK, it is necessary to create an SDK object that includes some configuration items in the following table. Please fill in according to the actual situation:

| configuration | description | method |
| ------------- | ----------- | ------ |
| RPC address | CESS chain rpc address | sdkgo.ConnectRpcAddrs |
| Mnemonic | CESS wallet mnemonic phrase, if empty, no transaction can be made. | sdkgo.Mnemonic |
| Transaction timeout | Timed out waiting for transaction completion, default is 30 seconds | sdkgo.TransactionTimeout |
| SDK name | It's just a name, default is `cess-sdk-go` | sdkgo.Name |

## Create a query-only sdk client
```golang
package main

import (
	"context"
	"fmt"

	sdkgo "github.com/CESSProject/cess-go-sdk"
)

var RPC_ADDRS = []string{
	//testnet
	"wss://testnet-rpc0.cess.cloud/ws/",
	"wss://testnet-rpc1.cess.cloud/ws/",
	"wss://testnet-rpc2.cess.cloud/ws/",
}

func main() {
	sdk, err := cess.New(
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

## Create a fully functional sdk client
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
	sdk, err := cess.New(
		context.Background(),
		sdkgo.ConnectRpcAddrs(RPC_ADDRS),
		sdkgo.Mnemonic(MY_MNEMONIC),
		sdkgo.TransactionTimeout(time.Second*10),
	)
	if err != nil {
		panic(err)
	}
	defer sdk.Close()
	fmt.Println(sdk.SystemVersion())
}
```