This is the interface to query the tee worker communication address.

```golang
// QueryEndpoints query tee's endpoint
//   - puk: tee's work public key
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - string: tee's endpoint
//   - error: error message
func (c *ChainClient) QueryEndpoints(puk WorkerPublicKey, block int32) (string, error)
```

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
		fmt.Println(sdk.QueryEndpoints(v.Pubkey, -1))
	}
}
```