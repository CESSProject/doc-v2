This is the interface for querying unique public keys across the network.

```golang
// QueryMasterPubKey query master public key
//   - block: block number, less than 0 indicates the latest block
//
// Return:
//   - []byte: master public key
//   - error: error message
func (c *ChainClient) QueryMasterPubKey(block int32) ([]byte, error)
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

    fmt.Println(sdk.QueryMasterPubKey(-1))
}
```