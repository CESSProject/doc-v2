This is the interface for querying the blocks registered by the tee worker.

```golang
// QueryWorkerAddedAt query tee work registered block
//   - puk: tee's work public key
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - uint32: tee work registered block
//   - error: error message
func (c *ChainClient) QueryWorkerAddedAt(puk WorkerPublicKey, block int32) (uint32, error)
```

For the type definition, please refer to [WorkerPublicKey](../chain_type.md#Type-definition)

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
		fmt.Println(sdk.QueryWorkerAddedAt(v.Pubkey, -1))
	}
}
```