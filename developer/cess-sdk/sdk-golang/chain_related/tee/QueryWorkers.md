This is the interface to query tee worker information.

```golang
// QueryWorkers query tee work info
//   - puk: tee's work public key
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - WorkerInfo: tee worker info
//   - error: error message
func (c *ChainClient) QueryWorkers(puk WorkerPublicKey, block int32) (WorkerInfo, error)
```

For the type definition, please refer to [WorkerInfo](../chain_type.md#WorkerInfo)

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
    "wss://testnet-rpc.cess.cloud/ws/",
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

	allWorks, err := sdk.QueryAllWorkers(-1)
	if err != nil {
		panic(err)
	}

	for _, v := range allWorks {
		fmt.Println(sdk.QueryWorkers(v.Pubkey, -1))
	}
}
```