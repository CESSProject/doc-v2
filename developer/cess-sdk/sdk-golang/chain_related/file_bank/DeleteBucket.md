This is the interface used to delete storage buckets, only empty buckets can be deleted.

```golang
// DeleteBucket delete a bucket for owner
//   - owner: bucket owner account
//   - bucketName: bucket name
//
// Return:
//   - string: block hash
//   - error: error message
//
// Note:
//   - if you are not the owner, the owner account must be authorised to you
func (c *ChainClient) DeleteBucket(owner []byte, bucketName string) (string, error)
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

    fmt.Println(sdk.DeleteBucket(sdk.GetSignatureAccPulickey(), "my_bucket"))
}
```